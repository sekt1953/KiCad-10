# 🔍 Opsamling på din fulde Git-struktur

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
