# PCBWay Plug-in til KiCad

PCBWay Plug-in til KiCad er et fantastisk værktøj, der gør det muligt at eksportere alle produktionsfiler og få et direkte pristilbud på printplader (PCB) og komponentmontering (Assembly) med ét enkelt klik.

Da du bruger KiCad via et AppImage, fungerer plug-inet upåklageligt, da det installeres direkte i din brugers lokale KiCad-mappe (som deles med AppImage-versionen).

## 🚀 Hvad gør plug-inet?

Når du trykker på PCBWay-knappen i din KiCad PCB Editor, sker der følgende automatisk:

1. Genererer Gerber-filer og borefiler (Drill files).
2. Genererer stykliste (BOM) med dine komponenter.
3. Genererer komponent-placeringsfiler (Pick and Place / Centroid-filer).
4. Uploader alt til PCBWay, åbner din browser og udfylder automatisk din bestilling med de korrekte dimensioner, lag og specifikationer.

## 🛠️ Sådan installerer du det i KiCad (Også via AppImage)

Det er nemmest at installere direkte inde fra KiCad:

1. Åbn hovedvinduet i KiCad.
2. Klik på Plugin and Content Manager (det ikon, der ligner en lille papkasse/pakke).
3. Gå til fanen Plugins eller Fabrication Plugin.
4. Søg efter PCBWay.
5. Klik på Install, og tryk derefter på Apply Pending Changes i bunden.
6. Genstart KiCad PCB Editor, hvis den var åben. Du vil nu se et PCBWay-logo i topmenuen.

## 💡 Vigtigt at huske i dit eget Git-bibliotek (kicad10)

For at PCBWays automatiske stykliste-overførsel (BOM) og montering virker fejlfrit, skal dine personlige symboler have de rigtige data tilknyttet.

Når du tegner symboler i dit nye kicad10 Git-repository, bør du altid tilføje disse felter (Fields) på dine komponenter:

* **MPN** (Manufacturer Part Number): Det præcise producent-varenummer (f.eks. CSTCE16M0V53-R0 eller NE555DR).
* **Manufacturer**: Producenten af chippen (f.eks. Texas Instruments eller Murata).

Hvis disse felter er udfyldt, kan PCBWay med det samme finde prisen på komponenterne i deres system, når du bruger plug-inet.

Vil du have hjælp til, hvordan du opsætter dine brugerdefinerede felter (Fields) i KiCads symbol-editor, så felter som MPN automatisk er klar, hver gang du laver et nyt symbol i dit repository?

## Sådan du opsætter dine brugerdefinerede felter (Fields)

For at gøre dit liv meget lettere, kan du konfigurere KiCad til automatisk at tilføje felter som MPN og Manufacturer, hver eneste gang du opretter et nyt symbol. På den måde glemmer du det ikke, og du slipper for at oprette felterne manuelt hver gang.

Da du bruger KiCad v10, gøres dette globalt i programmet, så det også virker, når du arbejder i dit kicad10 Git-repository.

Følg disse trin for at sætte det op:

## 🛠️ Trin-for-trin: Globale felter til nye symboler

1. Åbn hovedvinduet i KiCad.
2. Gå til Preferences -> Preferences... i topmenuen.
3. I venstre side af menuen folder du Symbol Editor ud og klikker på Default Fields.
4. Her kan du se de fire standardfelter (Value, Reference, Footprint, Datasheet).
5. Klik på + (Tilføj felt) i bunden af tabellen for at tilføje dine egne:
	• Tilføj et felt med navnet MPN (eller Manufacturer Part Number).
	• Tilføj endnu et felt med navnet Manufacturer.
6. (Valgfrit) Du kan fjerne fluebenet i kolonnen Show, hvis du ikke ønsker, at teksten skal være synlig oven i selve diagram-symbolet (værdierne vil stadig ligge gemt i baggrunden til din stykliste/BOM).
7. Klik OK for at gemme.

Når du nu åbner din Symbol Editor og klikker New Symbol, vil dine to nye felter stå klar i tabellen – tomme og klar til, at du indtaster komponentdata.
