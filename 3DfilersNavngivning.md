# Navngivning af 3D-modeller for KiCad

## 🧊 2. Navngivning af 3D-modeller

3D-modeller bruges til at vise dit printkort i 3D-visningen. Det bedste format at bruge i dag er .step (eller .stp), da det kan læses af stort set alle CAD-programmer (FreeCAD, Fusion 360 osv.). KiCad understøtter også .wrl (VRML), men .step er den professionelle standard.

* Struktur i dit repository: kicad10/3dmodels/
* Navngivningsregel: En rigtig god tommelfingerregel er, at 3D-filen skal hedde nøjagtig det samme som dit footprint (bortset fra filendelsen).
  * Footprint: Resistor_SMD_0805_2012Metric.**kicad_mod**
  * 3D-model: Resistor_SMD_0805_2012Metric.**step**

Når de hedder det samme, er det ekstremt nemt at parre dem inde i KiCad, og du er aldrig i tvivl om, hvilken model der hører til hvilket footprint.
