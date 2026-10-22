# Biblioteks navngivning

## Navngivning af Symbol filer

### 🎨 1. Navngivning af Symboler (Enkeltstående filer)

I modsætning til footprints er et symbol-bibliotek én enkelt fil, som indeholder alle de symboler, du tegner i det bibliotek. Filen skal ende på .kicad_sym.

* Struktur i dit repository: kicad10/symbols/MitEgetBibliotek.kicad_sym
* Navngivning af selve symbolet (inde i filen):
  * Brug producentens præcise modelnavn (f.eks. NE555DR, LM7805 eller STM32F103C8T6).
  * Undgå generiske navne som "Timer" eller "Processor", da det hurtigt bliver uoverskueligt, når dit bibliotek vokser.
  * Brug store bogstaver (caps), da det gør dem nemmere at læse i diagrammet.

## Navngivning af FootPrint filer

I KiCad har footprint-filer en meget specifik og fastlagt navngivningsstruktur, som programmet kræver for at kunne læse dem korrekt.

Kort sagt skal selve biblioteket være en mappe, der ender på .pretty, og de individuelle footprints indeni skal ende på .kicad_mod.
Her er præcis, hvordan du skal navngive dem i dit kicad10 Git-repository:

### 📁 1. Navngivning af biblioteksmappen (Skal ende på .pretty)

I KiCad er et footprint-bibliotek ikke én enkelt fil, men derimod en helt almindelig mappe på din computer. KiCad genkender kun mappen som et bibliotek, hvis navnet slutter med .pretty.

* Rigtigt: symbols_og_footprints/footprints/MitEgetBibliotek.pretty/
* Forkert: symbols_og_footprints/footprints/MitEgetBibliotek/

### 📄 2. Navngivning af de enkelte footprints (Skal ende på .kicad_mod)

Inde i din .pretty-mappe gemmes hvert eneste footprint som sin egen separate fil. Disse filer skal have endelsen .kicad_mod.

* Rigtigt: SOIC-8_3.9x4.9mm_P1.27mm.kicad_mod
* Forkert: SOIC-8_footprint.txt (eller uden endelse)

### 🏷️ Best practice: Hvordan skriver man selve navnet?

For at dine footprints er nemme at søge frem, og for at undgå problemer mellem forskellige operativsystemer (f.eks. hvis du deler dit Git-repository med en, der bruger Windows), bør du følge disse tre gyldne regler:

1. Brug kun standardtegn: Brug bogstaver (A-Z), tal (0-9), understreg (_) og bindestreg (-).
2. Undgå specialtegn og æ, ø, å: Undgå /, \, :, *, ?, 空格 (mellemrum) og danske tegn. Brug _ i stedet for mellemrum (f.eks. USB_C_Connector i stedet for USB C Connector).
3. Inkludér de vigtigste dimensioner: Følg KiCads egen standard ved at skrive pakketype, fysisk størrelse og ben-afstand (pitch) i navnet.
   * Eksempel: Resistor_SMD_0805_2012Metric (Komponenttype, pakkestørrelse i tommer, pakkestørrelse i mm).
   * Eksempel: Diodes_SMD_SOD-123

Når du opretter og gemmer et nyt footprint inde i KiCads Footprint Editor, sørger programmet automatisk for at give filen den rigtige .kicad_mod-endelse – du skal blot sikre dig, at du gemmer den i en mappe, der hedder noget med .pretty.

Vil du have et par eksempler på, hvordan du bedst navngiver specifikke komponenter, du skal i gang med at lave (f.eks. stik, IC'er eller modstande), så det matcher KiCads officielle officielle standard?

## Navngivning af 3D-modeller for KiCad

### 🧊 2. Navngivning af 3D-modeller

3D-modeller bruges til at vise dit printkort i 3D-visningen. Det bedste format at bruge i dag er .step (eller .stp), da det kan læses af stort set alle CAD-programmer (FreeCAD, Fusion 360 osv.). KiCad understøtter også .wrl (VRML), men .step er den professionelle standard.

* Struktur i dit repository: kicad10/3dmodels/
* Navngivningsregel: En rigtig god tommelfingerregel er, at 3D-filen skal hedde nøjagtig det samme som dit footprint (bortset fra filendelsen).
  * Footprint: Resistor_SMD_0805_2012Metric.**kicad_mod**
  * 3D-model: Resistor_SMD_0805_2012Metric.**step**

Når de hedder det samme, er det ekstremt nemt at parre dem inde i KiCad, og du er aldrig i tvivl om, hvilken model der hører til hvilket footprint.
