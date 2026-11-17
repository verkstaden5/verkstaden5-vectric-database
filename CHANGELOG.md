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

## [2.8.1] — 2026-11-17

### Fixed

- **Länkarna i de tyska och engelska filerna gick till sidor som inte finns.**
  Butikens tyska och engelska marknader är inte öppna än, så
  `/de-de/…` och `/en-eu/…` gav 404. Produktlänkarna i Fusion och
  sortimentslänken i Vectric går nu till butiken på verkstaden5.com tills
  marknaderna öppnar. Inga skärdata ändras.

---

## [2.8.0] — 2026-11-17

### Added

- **Hela sortimentet finns med: 120 fräsar, sju nya.** De läggs sist
  (verktygsnummer 114–120), så inga befintliga nummer ändras.

  | Nr | Verktyg | Vad |
  |---|---|---|
  | 114 | `DC-1F-D5.0-L15-C10` | 1-skärig nedåtskärande, 5 mm, 6 mm skaft |
  | 115 | `UC-PL-1F-D5.0-L17-C1b` | 1-skärig O-flute för plast, 5 mm, 6 mm skaft |
  | 116 | `V-D6.0-TD0.1-90°-F12` | V-bit 90° |
  | 117 | `DC-2F-D0.6-L3-C9` | 2-skärig nedåtskärande, 0,6 mm |
  | 118 | `UC-PL-1F-D1.5-L6-B10` | 1-skärig O-flute för plast, 1,5 mm |
  | 119 | `SB-3F-D25.4-F11` | 3-skärig planfräs, 25,4 mm |
  | 120 | `VG-D6.0-90°-F6` | V-spårfräs 90° för vikning av ACM |

- **Skärdata för 0,6 och 1,5 mm** i trä och plast. Värdena är 2 mm-värdena
  skalade linjärt med diametern och ligger i nedre halvan av Amanas tabell
  för små fräsar, till exempel 0,015 mm/tand och 360 mm/min i hårt trä för
  0,6 mm. Matningskollen visar nu också 0,6 och 1,5 mm för trä och plast.
- **Materialet ACM / Dibond**, bara för V-spårfräsen: 0,038 mm/tand vid
  16 000 rpm (1216 mm/min), 3 mm djup i ett pass på industrimaskin och två på
  hobbymaskin. Värdet kommer från Amanas tabell för V-spårfräsar till ACM.
  ACM visas inte i Matningskollen, eftersom det inte finns någon modell för
  andra fräsar i ACM.
- **Lättare att hitta rätt fräs i Fusion.** Beskrivningen är varukoden följd
  av gruppens namn på ditt språk, till exempel
  `UC-AL-DLC-1F-D3.175-L12-A9 · Aluminiumfräsar, 1-skäriga, DLC`, så att
  sökfältet hittar fräsen på "aluminium", "V-bit" eller "kulfräs". Varukoden
  ligger även i *Product id*, och *Product link* går till produktsidan i
  butiken på samma språk.
- Vectric-databasens grupper har svenska namn i den svenska filen
  (till exempel "Aluminiumfräsar, 1-skäriga" i stället för
  "Up cut spiral aluminium").
- Kontroll 17: varje verktyg har en produkt i butiken (`source/produkter.csv`).

### Fixed

- **Fusion-biblioteket har nu skärdjup och stegbredd.** Varje preset fick
  tidigare `use-stepdown` och `use-stepover` avslagna, så Fusion tog djupet
  från verktygsbanan. Hobbyprofilens halverade skärdjup och smalare stegbredd
  nådde därför bara Vectric-användarna. Nu får Fusion samma värden som Vectric,
  i det format Fusions egna exempelbibliotek använder. Exempel: 1-skärig
  DLC-fräs 3,175 mm i aluminium får 0,437 mm djup och 0,952 mm stegbredd i
  hobbyprofilen, 0,873 och 1,588 mm i den industriella.
- Kontroll 10 jämför också skärdjup och stegbredd mellan Vectric och Fusion,
  och fallerar om de är avslagna i Fusion.
- **V-bitar och koniska kulfräsar ritas rätt i Fusion.** De nio verktygen
  (`V-…` och `TB-…`) visade en varningstriangel och ingen skärdata, eftersom
  geometrin inte gick ihop: skärlängden var 18 mm oavsett vinkel, V-bitarna
  saknade spetsdiameter, och de koniska fräsarna hade skaftets diameter i
  spetsen och ingen kulradie. Nu räknas skärlängden ur konen, V-bitarna får
  `tip-diameter`, och de koniska får spetsdiameter, kulradie och
  `tapered-type: tapered_ball` enligt Fusions eget format. Verifierat genom
  import i Fusion 2026-11-17.

- **De koniska radiefräsarnas vinkel.** Alla fyra hade 20° per sida, efter
  SKU:n. Butiken anger vinkeln per sida och den skiljer sig per radie: 6,3°
  (R0,25), 5,8° (R0,5), 5,3° (R0,75) och 4,8° (R1,0). Med 20° trodde
  CAM-programmet att konen var mycket bredare än den är, vilket ger fel
  marginal mot branta väggar i 3D-banor. Gäller både Vectric (hel vinkel nu
  12,6°–9,6° i stället för 40°) och Fusion. Kontroll 3 jämför de koniska
  fräsarna mot butikens vinkel i stället för SKU:n.

- **V-bitarnas spetsvinkel var dubbelt så stor som den ska.** Vinkeln i SKU:n
  och på hemsidan är hel spetsvinkel, inte halv (butiken säljer även en
  90°-bit, som skulle vara platt med halv vinkel). Rättelsen i 2.0.0 gick åt
  fel håll. Nu gäller:

  | Verktyg | Hel vinkel före | Hel vinkel nu |
  |---|---|---|
  | `V-D3.175-TD0.1-20°-F1`, `…-TD0.2-20°-F2` | 40° | 20° |
  | `V-D6.0-TD0.1-30°-F3`, `…-TD0.2-30°-F4` | 60° | 30° |
  | `V-D6.0-TD0.1-60°-F5` | 120° | 60° |

  Vid V-carving räknar CAM-programmet djupet ur vinkeln, så med för stor
  vinkel blev spåren för grunda för en given bredd.
- `V-D6.0-TD0.1-60°-F5` följer nu samma matningstabell som övriga 6 mm
  V-bitar. Tabellen `matning_120` är borttagen; den gällde bara när F5
  räknades som 120°. Matningen i hårt trä går från 1800 till 2500 mm/min
  (hobby), i nivå med 2450 mm/min som F5 hade i originaldatabasen.

### Notes

För befintliga verktyg är varvtal, matning och chipload oförändrade utom för
`V-D6.0-TD0.1-60°-F5`. Planfräsen `SB-3F-D25.4-F11` har 3 skär på riktigt,
till skillnad från `SB-3F-D22` och `SB-3F-D30`, som har 2.
Fusion visar den koniska fräsens diameter där konen, förlängd, når spetsens
plan (till exempel Ø0,448 mm för R0,25). Det är Fusions definition; kulan
i spetsen är fortfarande Ø0,5 mm.

**Har du sparade 3D-banor med en konisk radiefräs (`TB-…`) eller
V-carving-banor med en V-bit (`V-…`) behöver de räknas om**, eftersom
verktygens form har ändrats.

Fusion visar vinkeln per sida i verktygslistan, så en 60° V-bit står som
"30°" och en 20° V-bit som "10°". Varukoden inom parentes är oförändrad.

---

## [2.7.0] — 2026-11-11

### Added

- **Butikens fräsförslag ligger i källan.** Nya `source/rekommendationer.yaml`
  anger vilka fräsar butiken föreslår per material och operation (profil och
  spår, ficka, gravyr, 3D). Bygget skriver `dist/verktygsval.json`, och
  releasen skriver den till shop-metafältet `verkstaden5.verktygsval`.
  Frässtålsväljaren och Matningskollen läser därifrån, så de två verktygen
  föreslår samma fräsar och går att köpa direkt.
- Kontroll 16: varje material i `chiploads.yaml` har förslag, och inga okända
  material eller operationer finns.

### Notes

Inga ändringar i skärdatan. Databasfilerna har samma innehåll som i 2.6.0,
och bara versionsnumret i Vectric-databasens anteckning skiljer sig.
Produktmetafälten `custom.skardata` får bara nytt versionsnummer.

---

## [2.6.0] — 2026-11-02

### Added

- **Engelska versioner av databaserna.** Varje release bygger nu även
  `verkstaden5-vectric-hobby-en.vtdb`, `verkstaden5-vectric-industriell-en.vtdb`,
  `verkstaden5-fusion360-hobby-en.tools` och
  `verkstaden5-fusion360-industriell-en.tools`. Materialnamn, verktygsgrupper
  och anteckningar är på brittisk engelska, med "router bit" och "chipload".
  Siffrorna är identiska med de övriga språkens filer.
- Shop-metafältet `verkstaden5.chiploads` innehåller `oversattning.en` för
  material och profiler, så att Feed Rate Calculator och produktsidornas
  skärdatatabeller visar engelska namn under `/en-eu`.

### Notes

Inga ändringar i skärdatans värden. De svenska, finska, danska, norska och
tyska filerna har samma innehåll som i 2.5.0, och bara versionsnumret i
Vectric-databasens anteckning skiljer sig. Produktmetafälten `custom.skardata`
får bara nytt versionsnummer.

---

## [2.5.0] — 2026-11-02

### Added

- **Tyska versioner av databaserna.** Varje release bygger nu även
  `verkstaden5-vectric-hobby-de.vtdb`, `verkstaden5-vectric-industriell-de.vtdb`,
  `verkstaden5-fusion360-hobby-de.tools` och
  `verkstaden5-fusion360-industriell-de.tools`. Materialnamn, verktygsgrupper
  och anteckningar är på tyska, och chipload heter "Zahnvorschub". Siffrorna
  är identiska med de svenska, finska, danska och norska filerna.
- Shop-metafältet `verkstaden5.chiploads` innehåller `oversattning.de` för
  material och profiler, så att Vorschubrechner och produktsidornas
  skärdatatabeller visar tyska namn under `/de-de`.

### Notes

Inga ändringar i skärdatans värden. De svenska, finska, danska och norska
filerna har samma innehåll som i 2.4.0, och bara versionsnumret i
Vectric-databasens anteckning skiljer sig. Produktmetafälten `custom.skardata`
får bara nytt versionsnummer.

---

## [2.4.0] — 2026-11-02

### Added

- **Norska versioner av databaserna (bokmål).** Varje release bygger nu även
  `verkstaden5-vectric-hobby-nb.vtdb`, `verkstaden5-vectric-industriell-nb.vtdb`,
  `verkstaden5-fusion360-hobby-nb.tools` och
  `verkstaden5-fusion360-industriell-nb.tools`. Materialnamn, verktygsgrupper
  och anteckningar är på norska. Siffrorna är identiska med de svenska, finska
  och danska filerna.
- Shop-metafältet `verkstaden5.chiploads` innehåller `oversattning.nb` för
  material och profiler, så att Matningskollen och produktsidornas
  skärdatatabeller visar norska namn under `/nb-no`.

### Notes

Inga ändringar i skärdatans värden. De svenska, finska och danska filerna har
samma innehåll som i 2.3.0, och bara versionsnumret i Vectric-databasens
anteckning skiljer sig. Produktmetafälten `custom.skardata` får bara nytt
versionsnummer.

---

## [2.3.0] — 2026-11-01

### Added

- **Danska versioner av databaserna.** Varje release bygger nu även
  `verkstaden5-vectric-hobby-da.vtdb`, `verkstaden5-vectric-industriell-da.vtdb`,
  `verkstaden5-fusion360-hobby-da.tools` och
  `verkstaden5-fusion360-industriell-da.tools`. Materialnamn, verktygsgrupper
  och anteckningar är på danska. Siffrorna är identiska med de svenska och
  finska filerna.
- Shop-metafältet `verkstaden5.chiploads` innehåller `oversattning.da` för
  material och profiler, så att Matningskollen och produktsidornas
  skärdatatabeller visar danska namn under `/da-dk`.

### Notes

Inga ändringar i skärdatans värden. De svenska och finska filerna har samma
innehåll som i 2.2.0, och bara versionsnumret i Vectric-databasens anteckning
skiljer sig. Produktmetafälten `custom.skardata` får bara nytt versionsnummer.

---

## [2.2.0] — 2026-11-01

### Added

- **Finska versioner av databaserna.** Varje release bygger nu även
  `verkstaden5-vectric-hobby-fi.vtdb`, `verkstaden5-vectric-industriell-fi.vtdb`,
  `verkstaden5-fusion360-hobby-fi.tools` och
  `verkstaden5-fusion360-industriell-fi.tools`. Materialnamn, verktygsgrupper
  och anteckningar är på finska. Siffrorna är identiska med de svenska filerna,
  eftersom båda språken räknas fram av samma `berakna()`.
- `source/sprak.yaml` med filernas texter per språk, plus `oversattning` för
  materialen i `chiploads.yaml` och profilerna i `machines.yaml`. Ett nytt
  språk läggs till där, utan ändringar i byggskripten.
- Shop-metafältet `verkstaden5.chiploads` innehåller `oversattning` för
  material och profiler, så att Matningskollen och produktsidornas
  skärdatatabeller visar namnen på butikens språk.
- Kontroll 15: varje material, profil och verktygsgrupp har namn på alla
  språk. Den fångar svenska namn som annars skulle hamna i en översatt fil.

### Notes

Inga ändringar i skärdatans värden. De svenska filerna har samma innehåll som
i 2.1.1, och bara versionsnumret i Vectric-databasens anteckning skiljer sig.
Produktmetafälten `custom.skardata` får bara nytt versionsnummer.

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
