# KiCad 10.0

## Dokumentation

* [KiCad's Dokumentation](./KiCad-10-0_Dokumentation.md#kicads-dokumentation)
* [YouTube om KiCad](./KiCad-10-0_Dokumentation.md#youtube-om-kicad)
  * [Skematisk design](./KiCad-10-0_Dokumentation.md#skematisk-design)
  * [PCB Design](./KiCad-10-0_Dokumentation.md#pcb-design)
  * [FreeCAD + KiCAD complete walkthrough](./KiCad-10-0_Dokumentation.md#freecad--kicad-complete-walkthrough)
  * [KiCad & EMI](./KiCad-10-0_Dokumentation.md#kicad--emi)
  * [KiCad General](./KiCad-10-0_Dokumentation.md#kicad-general)

## Libraries, Ubuntu & Appimages

* [Library Locations On Ubuntu](./LibraryLocationsOnUbuntu.md)
* [🛠️ Sådan fortæller du KiCad, hvor dine Library er](./SådanFortællerDuKiCadHvorLibraryEr.md)
* [Reset KiCad til default Setup](./ResetToDefaultSetup.md)
  * [To reset KiCad 10 to its default factory settings](./ResetToDefaultSetup.md#to-reset-kicad-10-to-its-default-factory-settings)
  * [Resetting Only the Libraries (Alternative)](./ResetToDefaultSetup.md#resetting-only-the-libraries-alternative)

## [Plugin](./Plugins.md)

* [PCBWay Plug-in til KiCad ](./PCBWay.md)
  * [🚀 Hvad gør plug-inet?](./PCBWay.md#-hvad-gør-plug-inet)
  * [🛠️ Sådan installerer du det i KiCad (Også via AppImage)](./PCBWay.md#️-sådan-installerer-du-det-i-kicad-også-via-appimage)
  * [💡 Vigtigt at huske i dit eget Git-bibliotek (kicad10)](./PCBWay.md#-vigtigt-at-huske-i-dit-eget-git-bibliotek-kicad10)
  * [Sådan du opsætter dine brugerdefinerede felter (Fields)](./PCBWay.md#sådan-du-opsætter-dine-brugerdefinerede-felter-fields)
  * [🛠️ Trin-for-trin: Globale felter til nye symboler](./PCBWay.md#️-trin-for-trin-globale-felter-til-nye-symboler)

## Navngivning af KiCad Librarie files

* Navngivning
  * [Symboler](./SymbolNavngivning.md)
  * [Footprint](./FootprintNavngivning.md)
  * [3d-modeller](./3DfilersNavngivning.md)

### 🔍 Opsamling på din fulde Git-struktur

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
