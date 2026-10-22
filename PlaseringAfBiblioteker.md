# Sådan fortæller du KiCad, hvor Library er

 Uanset hvor du gemmer dine filer, skal du registrere dem i KiCad, for at de dukker op i editoren:

1. Åbn KiCad.
2. Gå til Preferences -> Manage Symbol Libraries (for symboler) eller Manage Footprint Libraries (for footprints).
3. Vælg fanen Global Libraries (hvis de skal bruges i alle projekter) eller Project Specific Libraries (hvis de kun hører til det aktuelle projekt).
4. Klik på det lille mappe-ikon (+) i bunden for at tilføje dit bibliotek, og find frem til den mappe, hvor du har lagt dine filer.

Hvilken løsning passer bedst til dit behov? Hvis du vil, kan jeg hjælpe dig med at opsætte miljøvariabler (Environment Variables) i KiCad, så du nemt kan flytte dine biblioteker til en helt anden mappe (f.eks. i din Dokumenter-mappe eller Dropbox).

## Jeg valgt bruge et git reposotori kaldet kicad-10

At samle dine personlige footprints og symboler i et dedikeret Git-repository (f.eks. kaldet kicad10) giver dig versionsstyring, backup og gør det ekstremt nemt at synkronisere dine komponenter mellem forskellige computere.

Når du bruger et Git-repository, bør du ikke lægge det ind i den skjulte ~/.local/share/-mappe. Læg det i stedet et synligt sted i din hjemmemappe, for eksempel:
~/Udvikling/kicad10/ eller ~/Documents/kicad10/

For at gøre det super strømlinet i KiCad, anbefaler jeg, at du bruger Miljøvariabler (Environment Variables). Det gøres i tre enkle trin:

### 1. Opret strukturen i dit Git-repository

Sørg for at holde adskillelse på dine filer i dit repository, f.eks. sådan her:

* kicad10/symbols/MitBibliotek.kicad_sym
* kicad10/footprints/MitBibliotek.pretty/ (footprints ligger som .kicad_mod filer inde i denne mappe)
* kicad10/3dmodels/ (hvis du senere henter eller laver 3D-modeller)

### 2. Opret en brugerdefineret miljøvariabel i KiCad

Ved at lave en variabel slipper KiCad for at bruge absolutte søgestier (som f.eks. /home/bruger/...), hvilket gør det meget nemmere, hvis du kloner dit repository til en anden computer med et andet brugernavn.

1. Åbn KiCad AppImage.
2. Gå til Preferences -> Configure Paths...
3. Klik på + (Tilføj) i bunden af tabellen.
4. Giv variablen et navn i store bogstaver, f.eks. KICAD10_USER.
5. I feltet Path klikker du på mappen og vælger stien til dit Git-repository (f.eks. /home/ditnavn/Documents/kicad10).
6. Klik OK.

### 3. Tilføj bibliotekerne i KiCad

Nu skal du registrere dine biblioteker under Global Libraries, så de altid er tilgængelige:

1. Gå til Preferences -> Manage Symbol Libraries.
2. Sørg for, at du står på fanen Global Libraries.
3. Klik på + (Tilføj eksisterende).
4. Find dit symbol-bibliotek i dit Git-repository.
5. Smart trick: KiCad vil automatisk opdage din miljøvariabel og ændre stien i tabellen til ${KICAD10_USER}/symbols/MitBibliotek.kicad_sym.
6. Gør præcis det samme under Manage Footprint Libraries for dine .pretty-mapper.

Når du i fremtiden laver nye komponenter, gemmer du dem bare i dette repository, åbner din terminal, og kører de klassiske **git add, git commit og git push**.

## Gitignore her er et forslag til .gitignore

Her er et optimeret forslag til en .gitignore-fil, der passer perfekt til et globalt KiCad 10-biblioteks-repository.

Når du kun gemmer dine egne symboler, footprints og 3D-modeller (og ikke hele hardware-projekter), genererer KiCad ikke nær så meget "støj". Men der er stadig et par vigtige backup- og systemfiler, som du absolut bør filtrere fra for at holde dit repository rent.

### 📋 Den anbefalede .gitignore til dit bibliotek

Opret en fil med navnet .gitignore i rodmappen af dit kicad10-repository og indsæt følgende:

```ini
# KiCad backup- og midlertidige filer
*-bak
*.bak
*~

# Operativsystem-specifikke filer (skjulte filer fra filsystemet)
.DS_Store
Thumbs.db
.directory

# Eventuelle lokale cache-filer (hvis du åbner bibliotekseditoren direkte)
.fp-lib-table.swp
.sym-lib-table.swp
```

### 🔍 Hvorfor er disse linjer vigtige?

* ***-bak og *.bak**: Når du redigerer et footprint eller et symbol, gemmer KiCad automatisk den forrige version som en backup-fil (f.eks. MitBibliotek-bak). Dem vil du ikke have ind i Git, da Git i sig selv fungerer som din historik og backup.
* ***~**: Nogle teksteditorer og systemer laver midlertidige filer, der ender på en tilde.
* **.DS_Store / Thumbs.db / .directory**: Hvis du åbner mappen i din filstruktur på enten Linux (KDE/GNOME), Mac eller Windows, genererer operativsystemet ofte skjulte filer for at huske ikon-placeringer eller forhåndsvisninger. De hører ikke til i dit Git-repository.

### 💡 Et hurtigt tip til tomme mapper

Git tracker kun filer, ikke tomme mapper. Hvis du har oprettet en mappe til f.eks. 3dmodels/, men ikke har lagt noget i den endnu, vil Git ignorere mappen.

Hvis du gerne vil tvinge Git til at huske mappestrukturen med det samme, kan du oprette en tom, skjult fil kaldet **.gitkeep** inde i den tomme mappe.

## 🔍 Opsamling på din fulde Git-struktur

Nu hvor du har styr på alle tre elementer, vil din færdige og rene mappestruktur i dit kicad10 Git-repository se således ud:

```text
kicad10/
├── .gitignore                            <-- Den fælles filter-fil i roden
├── symbols/
│   └── Personlige_Komponenter.kicad_sym  <-- Én fil med alle symboler
├── footprints/
│   └── Personlige_Footprints.pretty/     <-- Mappe der slutter på .pretty
│       ├── SOIC-8_3.9x4.9mm_P1.27mm.kicad_mod
│       └── Resistor_SMD_0805.kicad_mod   <-- Individuelle footprints
└── 3dmodels/
    ├── SOIC-8_3.9x4.9mm_P1.27mm.step
    └── Resistor_SMD_0805.step            <-- 3D-filer med matchende navne
```

## Opsplitning af biblioteker efter kategorier

Det er faktisk en rigtig god idé at opdele dine biblioteker efter kategorier.

I KiCad behøver du nemlig ikke at have alle dine symboler i én stor fil. Du opretter bare to separate .kicad_sym filer i din symbols/-mappe. KiCad er fuldstændig ligeglad med, om et symbol er en lille modstand eller et stort færdigt printmodul (som f.eks. en Arduino Nano, en ESP32 eller en DC-DC konverter) – for KiCad er det bare to forskellige komponenter.

Her er, hvordan du bedst griber det an i dit Git-repository:

### 📁 Den nye struktur i din symbols/-mappe

Du opretter blot to forskellige filer i din mappen:

```text
kicad10/
└── symbols/
    ├── Komponenter.kicad_sym   <-- Til IC'er, modstande, kondensatorer, stik osv.
    └── PCB_Moduler.kicad_sym   <-- Til ESP32-boards, Step-down convertere, relæ-moduler osv.
```

### ⚙️ Sådan tilføjer du dem i KiCad

Når du har oprettet de to filer (eller gemt et symbol i dem for første gang), skal du blot registrere dem begge to i KiCad:

1. Gå til Preferences -> Manage Symbol Libraries.
2. Under Global Libraries klikker du på + (Tilføj) for at tilføje den første fil (Komponenter.kicad_sym).
3. Klik på + igen for at tilføje den næste fil (PCB_Moduler.kicad_sym).

Nu vil de optræde som to helt uafhængige kategorier i din komponent-liste, når du sidder og tegner dit diagram.

### 💡 En vigtig ting at huske med PCB-moduler

Når du laver symboler for PCB-moduler (hvor modulet i sig selv har ben, der skal loddes ned i dit eget printkort), skal du huske, at modulet også skal have et matchende footprint.
Du kan med fordel lave den samme opdeling i din footprints/-mappe, så du har:

* Komponenter.pretty/ (til almindelige SMD/THT-pakker)
* PCB_Moduler.pretty/ (til dine modulers fysiske dimensioner og ben-afstande)

### det samme gælder så også for 3d-modeller og footprint

Lige præcis! Det er den helt rigtige logik. For at holde dit system 100% strømlinet og symmetrisk, opdeler du bare både footprints og 3D-modeller på nøjagtig samme måde.
Når dine undermapper afspejler hinanden hele vejen igennem, bliver dit bibliotek ekstremt nemt at navigere i – både for dig selv og for KiCad.

#### 📁 Den komplette og professionelle Git-struktur

Dit kicad10 repository kommer til at se således ud, hvor alt er delt op i Komponenter og PCB_Moduler:

```text
Lib/
├── .gitignore
│
├── symbols/
│   ├── Komponenter.kicad_sym
│   └── PCB_Moduler.kicad_sym
│
├── footprints/
│   ├── Komponenter.pretty/
│   │   ├── Resistor_SMD_0805.kicad_mod
│   │   └── SOIC-8.kicad_mod
│   └── PCB_Moduler.pretty/
│       ├── Arduino_Nano.kicad_mod
│       └── ESP32_DevKitC.kicad_mod
│
└── 3dmodels/
    ├── Komponenter/
    │   ├── Resistor_SMD_0805.step
    │   └── SOIC-8.step
    └── PCB_Moduler/
        ├── Arduino_Nano.step
        └── ESP32_DevKitC.step
```

#### Oprettelse af biblioteker

```bash
mkdir -p ~/GitHub/KiCad-10/Lib/{symbols,footprints,3dmodels}/{Komponenter,PCB_Moduler}
```

### ⚙️ Det eneste du skal huske i KiCad

For at KiCad kan se dine nye opdelte footprints, skal du huske at tilføje begge .pretty-mapper under dine indstillinger:

1. Gå til Preferences -> Manage Footprint Libraries.
2. Tilføj både Komponenter.pretty og PCB_Moduler.pretty som to separate linjer i din Global Libraries-tabel.

### 🧊 Ekstra smart trick med 3D-modeller

Når du opretter undermapper i 3dmodels/ (som vist i strukturen ovenfor), gør din tidligere opsatte miljøvariabel (KICAD10_USER) det super nemt at linke til dem.

Når du inde i dit footprint skal vælge 3D-modellen, vil stien se således ud:

* Til en komponent: ${KICAD10_USER}/3dmodels/Komponenter/Resistor_SMD_0805.step
* Til et modul: ${KICAD10_USER}/3dmodels/PCB_Moduler/Arduino_Nano.step

Det spiller bare, og det er fuldstændig uafhængigt af, hvilken computer du åbner dit Git-repository på!
