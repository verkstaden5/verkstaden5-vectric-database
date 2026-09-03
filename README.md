# Verkstaden 5 — verktygsdatabas för Vectric

Officiell verktygsdatabas från [Verkstaden 5](https://verkstaden5.com) för
VCarve Desktop, VCarve Pro, Cut2D och Aspire. 113 hårdmetallfräsar med
skärparametrar för nio material.

## Ladda ner

Hämta senaste versionen under [Releases](https://github.com/verkstaden5/verkstaden5-vectric-database/releases/latest).
Välj profil efter din maskin:

| Fil | Passar |
|---|---|
| `verkstaden5-vectric-hobby.vtdb` | Skruv- eller remdriven maskin, trimfräs eller spindel upp till 1,5 kW |
| `verkstaden5-vectric-industriell.vtdb` | Styv portalfräs med servodrift och spindel från 2,2 kW |

Osäker? Börja med **hobby** och gå upp. En för låg startpunkt kostar tid, en
för hög kostar frässtål.

Hobbyprofilen sänker skärdjup och stegbredd, kapar matningen vid 2 500 mm/min
och sänker varvtalet om chiploaden skulle hamna så lågt att verktyget gnider i
stället för att skära.

## Installera

1. Ladda ner `.vtdb`-filen och spara den på datorn.
2. Öppna din Vectric-programvara. Gå till **Toolpaths → Tool Database**.
3. Klicka **Import** och välj filen.

Importen lägger till våra verktyg utan att skriva över dina befintliga. Du kan
sedan organisera dem i egna mappar.

## Vad ingår

**Verktyg:** uppåt-, nedåt-, kompressions- och notfräsar, kulfräsar, V-bitar,
koniska radiefräsar, plan- och avrundningsfräsar, kopierfräs och PCB-fräsar.

**Material:** hårt trä, mjukt träslag/plywood, MDF/spånskiva, melamin, akryl,
hård plast, mjuk plast, aluminium och FR4/PCB.

Varje verktyg har chipload, varvtal, matning, dykhastighet, skärdjup och
stegbredd per material.

## Om värdena

Detta är **startvärden för provfräsning**, inte facit. Justera efter din
maskin, uppspänning och material. Databasen levererar ingen maskinprofil —
använd din egen.

Ändringar per version finns i [CHANGELOG.md](CHANGELOG.md).

**Har du sparade verktygsbanor med `V-D6.0-TD0.1-60°-F5` behöver de räknas om
efter uppdatering till 2.0.0.** Spetsvinkeln var felaktigt angiven i tidigare
versioner och fräsdjupet blir ett annat med det rättade värdet.

## Kul att veta

Databasen genereras ur en gemensam källa tillsammans med vårt
[Fusion 360-bibliotek](https://github.com/verkstaden5/verkstaden5-fusion360-tool-library).
Samma verktyg får därför identiska värden i båda programmen — det kontrolleras
automatiskt vid varje release.

## Om Verkstaden 5

Din svenska partner för CNC-tillbehör. Vi fokuserar på frässtål som håller
längre och ger bättre resultat.

[verkstaden5.com](https://verkstaden5.com) · [Frässtål](https://verkstaden5.com/collections/frasverktyg) · [Verktygsdatabaser](https://verkstaden5.com/pages/verktygsdatabaser)

Sponsra gärna arbetet: https://github.com/sponsors/verkstaden5
