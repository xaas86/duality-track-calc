# Lumen Track

A single-file, dependency-free web app to calculate LED strip lengths and
plan walls for the **Duality LED Track System**.

Open `index.html` in any modern browser – no install, no server, no build
step. All work is stored locally in your browser (localStorage), with JSON
export/import for backup.

## Features

- **Strip list** – all 56 strip-carrying track parts with their path length
  in mm, a selectable quantity, the total length and the remainder against a
  spool length (default 5 m). Hovering a part name (on touch devices: tapping
  it) shows a schematic preview of its 35.8 mm band.
- **Printer filter** – pick a printer (Bambu Lab P1S, Bambu Lab A1, Snapmaker
  U1) and optionally show only parts that fit its build plate (flat or
  upright). The same selection drives the 3MF export.
- **Wall planner** – place parts to scale (35.8 mm wide bands) on a wall:
  drag & drop, 45° rotation, mirroring and magnetic snapping at free ends.
  Clone a part so it continues the run tangentially, undo/redo, zoom/pan,
  wall dimensions and grid, and export the plan as a **PNG image**.
- **Bill of materials** – turn the wall plan into the quantity table with one
  click.
- **JSON export/import** – save or restore the whole configuration.
- **3MF export** – generate a downloadable Bambu Studio project (`.3mf`) from
  your quantities or wall plan: choose a skin pattern and a printer; parts are
  laid on their largest face, tracks and skins go on separate, nested build
  plates, and the project uses neutral 0.20 mm standard settings. All model
  files stay in your browser – nothing is uploaded.
- **Bilingual UI** – switch the interface between German and English at any
  time.

## Usage

- **Online:** `https://xaas86.github.io/duality-track-calc/`.
- **Offline:** download `index.html` and open it directly in a browser.

## Notes and disclaimer

- **Early alpha release** – this app is under active development and may still contain bugs or incomplete features. Please test it and report any issues you find.
- Lengths are derived from the nominal dimensions in the part file names; for
  curved parts the values are rounded values based on the given radius and
  angle. Values may deviate slightly – please verify before ordering material.
- The wall planner draws a schematic center line of each track part.
- The 3MF export includes profiles for the Bambu Lab P1S, the Bambu Lab A1 and
  the Snapmaker U1 (from a project created in Snapmaker Orca). The printer
  selection also drives the printability filter.
- Not affiliated with the designer of the track system – see below.

## Credits

The **Duality Track System Bundle – Original Track and Evolution Pack** is a
design by **Andy Huot Creations**
(<https://www.printables.com/@AndyHuot_3193751>). The bundle covers all
tracks (Original and Evo); the tracks are also available as separate
projects:

- Bundle – Original Track and Evolution Pack:
  <https://www.printables.com/model/1637420-duality-track-system-bundle-original-track-and-evo>
- Track System (Original):
  <https://www.printables.com/model/1365700-duality-led-track-system>
- Evolution Pack:
  <https://www.printables.com/model/1637201-duality-evolution-pack>
- Skins (Creative Commons):
  <https://www.printables.com/model/1365694-creative-commons-duality-led-track-system-skins>

## License

Licensed under **Creative Commons Attribution-NonCommercial 4.0
International (CC BY-NC 4.0)** – free to use, share and adapt for
non-commercial purposes with attribution. Commercial use is not permitted.
See [`LICENSE`](LICENSE).

## Development

- `index.html` – the whole app (UI, geometry block, 3MF writer).
- `check.js` – self-check, no dependencies: `node check.js`.
- `CHANGELOG.md` – version history; the top entry must match `APP_VERSION`.
- **Versioning:** public releases aggregate the internal alpha versions (0.x.y-alpha); the public release number is the minor version (e.g. `0.3` covers all `0.3.x-alpha`).
- `measure_stls.py`, `build_export_data.py` – regenerate the static data
  block (`SKIN_MAP`, `PART_BBOX`, printer profiles) from local STL files.
- `publish.sh` – mirrors `index.html` + `CHANGELOG.md` + `README.md` into the
  public GitHub repo (only run on request).
- `AGENTS.md` – project contracts and conventions (German).

---

## Lumen Track (Deutsch)

Eine einzige, abhängigkeitsfreie HTML-Datei zum Berechnen von
LED-Streifenlängen und zum Planen von Wänden für das **Duality LED Track
System**.

`index.html` in einem modernen Browser öffnen – keine Installation, kein
Server, kein Build. Alle Daten bleiben lokal im Browser (localStorage),
Sichern/Wiederherstellen per JSON-Export/-Import.

### Funktionen

- **Streifenliste** – alle 56 streifenführenden Track-Teile mit Pfadlänge in
  mm, wählbarer Menge, Gesamtlänge und Rest gegenüber einer Rollenlänge
  (Standard 5 m). Beim Überfahren des Namens (auf Touch-Geräten: Antippen)
  erscheint eine schematische Vorschau des 35,8-mm-Bands.
- **Druckerfilter** – Drucker wählen (Bambu Lab P1S, Bambu Lab A1, Snapmaker
  U1) und optional nur Teile anzeigen, die auf dessen Platte passen (flach
  oder hochkant). Dieselbe Auswahl steuert den 3MF-Export.
- **Wandplaner** – Teile maßstäblich (35,8 mm breite Bänder) auf eine Wand
  legen: Drag & Drop, 45°-Drehung, Spiegeln und Magnet-Snapping an freien
  Enden. Klonen setzt ein Teil tangential fort, Rückgängig/Wiederholen,
  Zoom/Pan, Wandmaße und Raster, Export des Plans als **PNG-Bild**.
- **Stückliste** – den Wandplan mit einem Klick in die Mengentabelle
  übernehmen.
- **JSON-Export/-Import** – die gesamte Konfiguration sichern/wiederherstellen.
- **3MF-Export** – aus Stückliste oder Wandplan ein herunterladbares
  Bambu-Studio-Projekt (`.3mf`) erzeugen: Muster und Drucker wählen; Teile
  werden auf die größte Fläche gelegt, Tracks und Skins liegen auf getrennten,
  verschachtelt gepackten Platten, das Projekt nutzt neutrale
  0,20-mm-Standardwerte. Alle Modelldateien bleiben im Browser – es wird
  nichts hochgeladen.
- **Zweisprachige Oberfläche** – die Bedienoberfläche jederzeit zwischen
  Deutsch und English umschalten.

### Nutzung

- **Online:** `https://xaas86.github.io/duality-track-calc/`.
- **Offline:** `index.html` herunterladen und direkt im Browser öffnen.

### Hinweise und Haftung

- **Frühe Alpha-Version** – die App befindet sich in aktiver Entwicklung und kann noch Fehler oder unvollständige Funktionen enthalten. Bitte testen und Probleme melden.
- Die Längen werden aus den Nennmaßen der Dateinamen abgeleitet; bei
  gebogenen Teilen handelt es sich um Rundungswerte anhand von Radius und
  Winkel. Die Werte können geringfügig abweichen – bitte vor der
  Materialbestellung prüfen.
- Der Wandplaner zeichnet eine schematische Mittellinie je Track-Teil.
- Der 3MF-Export enthält Profile für Bambu Lab P1S, Bambu Lab A1 und Snapmaker
  U1 (aus einem in Snapmaker Orca erzeugten Projekt). Dieselbe Druckerwahl
  steuert den Druckbarkeits-Filter.
- Kein Zusammenhang mit dem Designer des Systems – siehe unten.

### Danksagung / Credits

Das **Duality Track System Bundle – Original Track and Evolution Pack** ist
ein Design von **Andy Huot Creations**
(<https://www.printables.com/@AndyHuot_3193751>). Das Bundle umfasst alle
Tracks (Original und Evo); die Tracks gibt es auch als einzelne Projekte:

- Bundle – Original Track and Evolution Pack:
  <https://www.printables.com/model/1637420-duality-track-system-bundle-original-track-and-evo>
- Track System (Original):
  <https://www.printables.com/model/1365700-duality-led-track-system>
- Evolution Pack:
  <https://www.printables.com/model/1637201-duality-evolution-pack>
- Skins (Creative Commons):
  <https://www.printables.com/model/1365694-creative-commons-duality-led-track-system-skins>

### Lizenz

Lizenziert unter **Creative Commons Namensnennung – Nicht kommerziell 4.0
International (CC BY-NC 4.0)** – kostenlose Nutzung, Weitergabe und
Bearbeitung für nicht-kommerzielle Zwecke mit Namensnennung. Kommerzielle
Nutzung ist nicht gestattet. Siehe [`LICENSE`](LICENSE).

### Entwicklung

- `index.html` – die gesamte App (UI, Geometrie-Block, 3MF-Writer).
- `check.js` – Selbstkontrolle ohne Abhängigkeiten: `node check.js`.
- `CHANGELOG.md` – Versionshistorie; der oberste Eintrag muss `APP_VERSION` entsprechen.
- **Versionierung:** Öffentliche Releases fassen die internen Alpha-Versionen (0.x.y-alpha) zusammen; die öffentliche Versionsnummer ist die Minor-Version (z. B. `0.3` umfasst alle `0.3.x-alpha`).
- `measure_stls.py`, `build_export_data.py` – erzeugen den statischen
  Datenblock (`SKIN_MAP`, `PART_BBOX`, Druckerprofile) aus lokalen STLs neu.
- `publish.sh` – spiegelt `index.html` + `CHANGELOG.md` + `README.md` ins
  öffentliche GitHub-Repo (nur auf Anweisung ausführen).
- `AGENTS.md` – Arbeitsvertrag und Konventionen.
