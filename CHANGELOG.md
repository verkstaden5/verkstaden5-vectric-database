# Changelog

Alla ändringar i Verkstaden 5:s verktygsdatabaser dokumenteras här. Detta är den
enda changelog som skrivs för hand — de som ligger i distributionsrepona
genereras härifrån.

Formatet följer [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) och
versionsnumret [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Från och med 2.0.0 delar Vectric- och Fusion-databaserna versionsnummer,
eftersom de byggs ur samma källa. Det var skillnaden mellan dem som gjorde att
den ena kunde bli fem månader gammal utan att någon märkte det.

---

## [2.1.1] — 2026-09-03

### Fixed

- **`shopify`-jobbet saknade `npm ci`.** `emit-product-data.mjs` importerar
  `calc.mjs`, som behöver `yaml`-paketet — men jobbet installerade aldrig
  beroenden (till skillnad från `bygg`-jobbet), eftersom `push-metafield.mjs`
  klarar sig utan. v2.1.0:s release misslyckades därför på steget "Skriv
  produktmetafälten". Källrepots release och båda distributionsrepona
  publicerades korrekt i v2.1.0 och behövde inte göras om.

---

## [2.1.0] — 2026-09-03

### Added

- **Produktsidornas skärdatatabell.** `build/emit-product-data.mjs` skriver ett
  metafält `custom.skardata` (JSON) per produkt, ett per artikelfamilj som
  täcker alla dess varianter. Metafältet lagrar varje verktygs naturliga,
  okapade chipload (industriell-profilen — den kapar aldrig) plus varvtal och
  skärantal; produktsidan räknar själv om till matning för valt varvtal och
  maskinprofil i webbläsaren, med ett fritt varvtalsfält precis som
  Matningskollens kalkylator. Produkter vars variant-SKU:er inte matchar
  `tools.csv`, eller vars verktyg är av typen `kopier` (kullagerstyrd
  kopieringsfräs för handhållen överfräs, inte avsedd för CNC), får inget
  metafält.
- Kontroll 14 i `tests/validate.mjs`: butikens produktmetafält matchar det
  byggda. Hoppas över lokalt om Shopify-uppgifterna saknas, precis som
  kontroll 13.
- Release-workflowet skriver nu produktmetafälten och verifierar dem som en
  del av `shopify`-jobbet, efter shop-metafältet `verkstaden5.chiploads`.

### Notes

Inga ändringar i skärdatans värden. Metafältsdefinitionen `custom.skardata`
(typ `json`, storefront `PUBLIC_READ`) skapades manuellt i butiken innan
första körningen.

---

## [2.0.1] — 2026-09-03

### Notes

Testrelease för att verifiera hela publiceringskedjan: bygge, regelkontroller,
release i de två distributionsrepona och skrivning till shop-metafältet
`verkstaden5.chiploads`. Inga ändringar i skärdatan.

---

## [2.0.0] — 2026-09-03

Databaserna byggs nu ur en gemensam källa i stället för att underhållas för hand
i Vectric respektive Fusion. Släppet innehåller flera rättelser av fel som fanns
i tidigare versioner, varav ett gav synligt felaktiga fräsdjup.

### Fixed

- **V-bitarnas spetsvinkel.** `V-D6.0-TD0.1-60°-F5` låg med 60 graders hel
  spetsvinkel i Vectric-databasen. SKU:n anger **halv** vinkel, så det korrekta
  värdet är 120 grader hel. Vid V-carving beräknar CAM-programmet fräsdjupet ur
  spetsvinkeln — med fel vinkel blev djupet ungefär tre gånger för stort för en
  given spårbredd. Fusion hade rätt värde (`TA: 60`, halv vinkel) och påverkades
  inte.
- **Dublett av V-bitsgeometri.** En andra geometri med namnet
  `V-D6.0-TD0.2-30°-F4` men 120 graders vinkel låg i Vectric-databasen utan
  motsvarande artikel i sortimentet. Borttagen. Detta löser även den kända
  buggen från Fusion 1.1.0 där två verktyg delade namn.
- **PCB-fräsarna saknade skärdata i Vectric.** Alla sex fanns i verktygsträdet
  men hade inga parametrar alls — samma defekt som fick Fusion 1.0 att vägra
  importeras. De har nu FR4-värden i båda databaserna.
- **Fel skärlängd på `UC-AL-DLC-1F-D5.0-L10-B5b`.** Verktyget har 12 mm
  skärlängd, inte 10. Omdöpt till `UC-AL-DLC-1F-D5.0-L12-B5b` och måttet rättat.
- **Skaftdiametern var fel på 28 av 113 verktyg** i Fusion-biblioteket, som satt
  skaftet lika med skärdiametern. Samtliga 2 mm-fräsar sitter på 3,175 mm skaft,
  plan- och avrundningsfräsarna på 6 mm och kopierfräsen på 6,35 mm. Måtten
  hämtas nu ur butikens artikeldata. Skaftdiametern används av CAM-programmen
  för kollisionskontroll mot uppspänning och fixturer.
- **Motstridiga värden på 6 mm kulfräs.** `BN-2F-D6.0-L32-R3-C7b` och
  `BN-2F-D6.0-L12-R3-B8`/`-L22-R3-B9` hade dubbelt så hög matning som varandra
  trots identisk geometri. Se *Changed* nedan.
- **Chipload minskade med växande diameter** för sex av sju material mellan 2 mm
  och 3,175 mm. Se *Changed* nedan.

### Changed

- **Kulfräsarnas chipload följer nu effektiv diameter.** En kulfräs skär inte med
  full diameter: vid skärdjupet `ap` är den effektiva diametern
  `Deff = 2 · √(ap · (D − ap))`. Chiploaden hämtas nu vid `Deff` i stället för
  vid den nominella diametern. Tidigare använde 2 mm och 4 mm pinnfräsvärdet
  rakt av, 3,175 mm låg något under och 6 mm hade två olika värden. Ändringen
  berör samtliga kulfräsar och sänker chiploaden i de flesta fall.
- **2 mm-värdena omräknade.** Chipload ska växa med diametern. De gamla
  2 mm-värdena låg över 3,175 mm-värdena för sex av sju material. De är omräknade
  ur varje materials egen potenskurva, anpassad mot mätpunkterna från 3,175 mm
  och uppåt. Hårt trä går från 0,090 till 0,043–0,057 mm/tand.
- **Dykhastigheten är inte längre 50 % av matningen rakt igenom.** Dyket är den
  hårdaste belastningen ett verktyg utsätts för. Nya andelar: trä och MDF 35 %,
  plast 25 %, aluminium 20 %, gravyr 25 %, kulfräs 30 %, FR4 30 %. Använd ramp
  eller helix där ditt CAM-program stöder det.
- **Databasen levererar bandets mittvärde** som startvärde i stället för ett
  värde ur intervallets övre del. Hela intervallet visas i stället i
  Matningskollen på verkstaden5.com.
- **Skärdjupet vid 3,175 mm är normaliserat.** Vissa verktyg räknade på 3,0 mm
  i stället för 3,175 mm, vilket gav 1,5 mm skärdjup där andra fick 1,5875 mm.

### Added

- **Två maskinprofiler.** `industriell` motsvarar tidigare värden och förutsätter
  en styv maskin med spindel på minst 2,2 kW. `hobby` är anpassad för skruv- och
  remdrivna maskiner: skärdjupet halveras, stegbredden går till 60 %, matningen
  kapas vid 2 500 mm/min och dyket vid 800 mm/min.

  Ordningen är avsiktlig. Kapar man bara matningen faller chiploaden och
  verktyget gnider i stället för att skära. Hamnar chiploaden under
  gnidningsgränsen sänks därför varvtalet i stället, ned till 10 000 rpm.
- **Melamin** som eget material, med samma värden som MDF/spånskiva.
- **FR4 / PCB** som material i Vectric-databasen. Fanns tidigare bara i Fusion.
- **Tre verktyg som saknades** trots att de säljs: kopierfräsen
  `FT-2F-D12.7-L38-F10b` och avrundningsfräsarna `RBB-2F-D1/4-R1/4-F1b` och
  `RBB-2F-D1/4-R3/16-F2b`. Värdena är härledda ur modellen, inte provfrästa.

### Removed

- **Maskinprofilen `WorkBee Z1+ 1000x1500`.** Databasen levererade en enda
  maskinprofil som 42 procent av matningarna överskred maxhastigheten för.
  Ingen maskinprofil levereras längre — använd din egen.
- **28 tomma poster** i Vectric-databasen utan material och utan parametrar.
  Verktygen finns kvar; det var dubblettrader av verktyg som också hade
  fungerande skärdata.

### Migration

Verktygsnamnen är oförändrade utom `UC-AL-DLC-1F-D5.0-L10-B5b`, som heter
`…-L12-B5b`. Importerar du över en tidigare version ligger det gamla namnet
kvar och kan tas bort manuellt.

**Har du sparade verktygsbanor med `V-D6.0-TD0.1-60°-F5` behöver de räknas om.**
Spetsvinkeln var fel och fräsdjupet blir ett annat med det rättade värdet.

Välj profil vid nedladdningen. Är du osäker: börja med `hobby` och gå upp.

### Notes

Webbplatsen har angett "v1.3 – uppdaterad juli 2026" för Vectric-databasen. Den
versionen har aldrig funnits som fil. Den nedladdningsbara filen låg kvar på
versionen från 2026-04-27, byte-identisk med den i GitHub-repot. Numreringen
börjar därför om på 2.0.0 för båda databaserna, gemensamt.

Samtliga värden är startvärden för provfräsning. Justera efter din maskin,
uppspänning och material.

---

## [1.1.0] — 2026-07-21 *(endast Fusion 360)*

### Fixed
- Biblioteket gick inte att importera efter Fusions julirelease 2026. Sex
  PCB-fräsar levererades med tom preset-lista, vilket fick hela filen att
  avvisas.

### Added
- FR4/PCB-preset för de sex PCB-fräsarna, avstämt för 24 000 rpm.

---

## [1.0.0] — 2026-04-28

### Added
- Första publicerade versionen: 111 hårdmetallverktyg med skärdata för trä,
  plast, komposit och aluminium.
