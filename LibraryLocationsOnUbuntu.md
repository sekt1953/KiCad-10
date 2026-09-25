# Library Locations on Ubuntu

## Sådan tjekker du, hvor KiCad kigger lige nu

Hvis du vil se nøjagtigt, hvilke stier din specifikke KiCad-installation bruger som standard:

1. Åbn KiCad 10.
2. Gå til Preferences > Configure Paths...
3. Kig efter variablerne KICAD10_SYMBOL_DIR og KICAD10_FOOTPRINT_DIR. Stien ud for dem viser præcis, hvor programmet leder efter standardsymboler på din computer.

Ved du, om du installerede KiCad som en Flatpak/Snap eller via APT? Hvis du åbner Configure Paths, hvilken sti står der så ud for KICAD10_SYMBOL_DIR?

## tmp/.mount_kicadremp9728916709621493320/share/kicad/symbols/

Stien /tmp/.mount_kicad... afslører, at ***du kører KiCad som en AppImage!***  
Når du dobbeltklikker på en AppImage-fil, pakker Ubuntu programmet midlertidigt ud i en virtuel mappe under /tmp/. Denne mappe forsvinder fuldstændigt, hver eneste gang du lukker KiCad eller genstarter computeren.  
Det ændrer dog ikke på, hvor du skal gemme dine egne ting. Du skal fortsat holde dig langt væk fra den midlertidige mappe og i stedet bruge din egen hjemmemappe.

### Sådan skal du gøre med AppImage:

1. Placer dine egne filer i din hjemmemappe:  
Opret dine egne biblioteker i en fast mappe, der ikke forsvinder:

```bash
bash

mkdir -p ~/Documents/KiCad/10.0/{symbols,footprints,3dmodels}
```

2. Tilføj dem globalt i KiCad:
   Selvom selve programmet ligger i en midlertidig /tmp/-mappe, gemmer KiCad dine indstillinger i din rigtige hjemmemappe (~/.config/kicad/10.0/).
   1. Gå til Preferences > Manage Symbol Libraries...
   2. Tilføj dit eget bibliotek fra ~/Documents/KiCad/10.0/symbols/.
   3. KiCad vil huske denne sti, næste gang du starter din AppImage.

#### En vigtig detalje om AppImage:

* Da AppImage har alle standard-symboler og footprints "låst fast" inde i sin egen fil, kan du ikke ændre eller tilføje noget i systemets standardbiblioteker. Alt, hvad du selv laver eller downloader (fra f.eks. internettet), skal ligge i din ~/Documents/KiCad/-mappe.
* Har du fået oprettet dit eget bibliotek derinde, eller driller det med at få KiCad til at genkende stien permanent?
