# Navngivning af Symbol filer

## 🎨 1. Navngivning af Symboler (Enkeltstående filer)

I modsætning til footprints er et symbol-bibliotek én enkelt fil, som indeholder alle de symboler, du tegner i det bibliotek. Filen skal ende på .kicad_sym.

* Struktur i dit repository: kicad10/symbols/MitEgetBibliotek.kicad_sym
* Navngivning af selve symbolet (inde i filen):
  * Brug producentens præcise modelnavn (f.eks. NE555DR, LM7805 eller STM32F103C8T6).
  * Undgå generiske navne som "Timer" eller "Processor", da det hurtigt bliver uoverskueligt, når dit bibliotek vokser.
  * Brug store bogstaver (caps), da det gør dem nemmere at læse i diagrammet.
