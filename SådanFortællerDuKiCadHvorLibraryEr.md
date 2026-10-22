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
