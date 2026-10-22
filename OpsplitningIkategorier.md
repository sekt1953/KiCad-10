# Opsplitning af biblioteker efter kategorier

Det er faktisk en rigtig god idé at opdele dine biblioteker efter kategorier.

I KiCad behøver du nemlig ikke at have alle dine symboler i én stor fil. Du opretter bare to separate .kicad_sym filer i din symbols/-mappe. KiCad er fuldstændig ligeglad med, om et symbol er en lille modstand eller et stort færdigt printmodul (som f.eks. en Arduino Nano, en ESP32 eller en DC-DC konverter) – for KiCad er det bare to forskellige komponenter.

Her er, hvordan du bedst griber det an i dit Git-repository:

## 📁 Den nye struktur i din symbols/-mappe

Du opretter blot to forskellige filer i din mappen:

```text
kicad10/
└── symbols/
    ├── Komponenter.kicad_sym   <-- Til IC'er, modstande, kondensatorer, stik osv.
    └── PCB_Moduler.kicad_sym   <-- Til ESP32-boards, Step-down convertere, relæ-moduler osv.
```

## ⚙️ Sådan tilføjer du dem i KiCad

Når du har oprettet de to filer (eller gemt et symbol i dem for første gang), skal du blot registrere dem begge to i KiCad:

1. Gå til Preferences -> Manage Symbol Libraries.
2. Under Global Libraries klikker du på + (Tilføj) for at tilføje den første fil (Komponenter.kicad_sym).
3. Klik på + igen for at tilføje den næste fil (PCB_Moduler.kicad_sym).

Nu vil de optræde som to helt uafhængige kategorier i din komponent-liste, når du sidder og tegner dit diagram.

## 💡 En vigtig ting at huske med PCB-moduler

Når du laver symboler for PCB-moduler (hvor modulet i sig selv har ben, der skal loddes ned i dit eget printkort), skal du huske, at modulet også skal have et matchende footprint.
Du kan med fordel lave den samme opdeling i din footprints/-mappe, så du har:

* Komponenter.pretty/ (til almindelige SMD/THT-pakker)
* PCB_Moduler.pretty/ (til dine modulers fysiske dimensioner og ben-afstande)

## det samme gælder så også for 3d-modeller og footprint

Lige præcis! Det er den helt rigtige logik. For at holde dit system 100% strømlinet og symmetrisk, opdeler du bare både footprints og 3D-modeller på nøjagtig samme måde.
Når dine undermapper afspejler hinanden hele vejen igennem, bliver dit bibliotek ekstremt nemt at navigere i – både for dig selv og for KiCad.

### 📁 Den komplette og professionelle Git-struktur

Dit kicad10 repository kommer til at se således ud, hvor alt er delt op i Komponenter og PCB_Moduler:

```text
kicad10/
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

## ⚙️ Det eneste du skal huske i KiCad

For at KiCad kan se dine nye opdelte footprints, skal du huske at tilføje begge .pretty-mapper under dine indstillinger:

1. Gå til Preferences -> Manage Footprint Libraries.
2. Tilføj både Komponenter.pretty og PCB_Moduler.pretty som to separate linjer i din Global Libraries-tabel.

## 🧊 Ekstra smart trick med 3D-modeller

Når du opretter undermapper i 3dmodels/ (som vist i strukturen ovenfor), gør din tidligere opsatte miljøvariabel (KICAD10_USER) det super nemt at linke til dem.

Når du inde i dit footprint skal vælge 3D-modellen, vil stien se således ud:

* Til en komponent: ${KICAD10_USER}/3dmodels/Komponenter/Resistor_SMD_0805.step
* Til et modul: ${KICAD10_USER}/3dmodels/PCB_Moduler/Arduino_Nano.step

Det spiller bare, og det er fuldstændig uafhængigt af, hvilken computer du åbner dit Git-repository på!
