# Changelog

Versionierung: SemVer mit Kanal-Suffix (`-alpha`, `-beta`, `-rc`); der Kanal wird in der App angezeigt.
Die oberste Version hier muss der Konstante `APP_VERSION` in `index.html` entsprechen (prüft `check.js`).
Derselbe Changelog ist in der App unter „Änderungen" eingeblendet.

## [0.3.6-alpha] – 2026-11-23

### Neu (Vorschau in der Streifenliste)

- **Hover-Vorschau:** Beim Überfahren des Teilenamens erscheint ein kleines Popover mit dem 35,8-mm-Band als schematische Grafik – Kategoriefarbe, gestrichelte Mittellinie (die gemessene Streifenlinie) und Maß im Untertitel. Gezeichnet wird bei Bedarf aus `shapeOf()` (dieselbe Geometrie wie im Wandplaner), also **ohne Bilddateien** und ohne Größenwachstum von `index.html`.
- **Touch:** Auf Geräten ohne Hover zeigt ein Tippen auf den Namen die Vorschau, erneutes Tippen oder ein Tipp außerhalb schließt sie.
- **Aufräumen:** Die Vorschau verschwindet beim Verlassen des Namens (nicht erst der Liste), beim Scrollen, bei Filter-/Sprachwechsel (Neu-Rendern) und beim Tab-Wechsel.
- `check.js` unverändert grün (56 Teile, 3 Drucker, 7 Muster).

## [0.3.5-alpha] – 2026-11-23

### Neu (Snapmaker-U1-Exportprofil)

- **U1 exportiert jetzt als 3MF:** `PRINTERS.u1` hat ein vollständiges Profil, 1:1 übernommen aus einem vom User in **Snapmaker Orca 2.4.2** erzeugten Projekt (`printer_model: Snapmaker U1`, 609 Keys, `print_settings_id: 0.20mm Standard @Snapmaker U1 (0.4 nozzle)`). Platte 270 × 270 × 270 mm, 0,4-mm-Düse. Der Download ist für den U1 nicht mehr gesperrt; der Druckbarkeits-Filter bleibt unverändert.
- **Neutral:** Das Orca-Projekt ist bereits neutral (auto_brim, Textured PEI, 0,2 mm). Einzige Abweichung beim Export ist `enable_support: '0'`. Die `different_settings_to_system`-Liste bleibt beim U1 unangetastet (Bambu-Profile werden weiterhin geleert) – der Key-Satz bleibt so 1:1.
- **Slicer-Kennung je Drucker:** Der Writer schreibt je Drucker `app`/`client`/`clientVer`. Beim U1 steht `Application = Snapmaker_Orca-2.4.2`, `BambuStudio:3mfVersion` entfällt, `slice_info.config` nutzt `X-SM-Client-*`. P1S/A1 behalten `BambuStudio-<Version>`, `BambuStudio:3mfVersion` und `X-BBL-Client-*`.
- **Profilquelle und Generator:** Das echte U1-Projekt liegt als `test-files/hear-see-speak-no-evil-santa-claus-gnomes-snapmaker-u1_own.3mf` im Repo. `build_export_data.py` hat `--u1` (Default: diese Datei) und prüft je Drucker, dass `printer_model` passt.
- **`check.js`:** prüft statt „U1 ohne Profil" jetzt Plattenmaß 270, Version 2.4.2, `printer_model`, ≥ 400 Keys, den neutralen Support-Wert, den unveränderten Key-Satz und die Slicer-Kennung im erzeugten 3MF (Bambu- und Orca-Fall, ZIP-Einträge entpackt).
- **Testdatei:** `test-u1-0.3.5-alpha.3mf` – Hex-Auswahl wie bei den 0.3.4-Testdateien: 14 Track- + 7 Skin-Teile (6× gerade 250, 1× 90°, 3× Tangente 90, 4× Tangente 45; 6× Skin gerade 250, 1× Skin 90°) auf 5 Platten (3 Track + 2 Skin), Track-Teile 21,8 mm, Skins 6,9 mm hoch, nichts hochkant; per Python validiert (ZIP/XML, `Snapmaker_Orca-2.4.2`, neutrales 609-Key-Profil, Platten innerhalb 270 mm, keine Baugruppen). User-Test in Snapmaker Orca offen.

## [0.3.4-alpha] – 2026-11-21

### Umbenennung

- Die App heißt jetzt **Lumen Track** (vorher „Duality LED Track – Streifenrechner“). Das System selbst bleibt das *Duality LED Track System* von Andy Huot; Ordner, Repo und Datei (`index.html`) sind entsprechend umbenannt.

### Behoben (Ausrichtung, Platten und Profile im 3MF-Export)

- **Größte Auflagefläche statt pauschaler Y-Drehung:** `layFlat()` drehte jedes Teil um die Y-Achse, wodurch Geraden, Tangenten und Skins hochkant auf der 21,8- bzw. 6,9-mm-Kante standen. Neu bestimmt `faceAreaByAxis()` die Flächenanteile je Achse, `layLargest()` legt die größte Fläche nach unten: Geraden/Tangenten/Skins auf die 35,8-mm-Fläche (Höhe 21,8 bzw. 6,9 mm), Down-/Up-Kurven auf die X-Fläche (35,8 mm) – deckungsgleich mit den Designer-3MFs und dem Bambu-Studio-Layout des Users.
- **Verschachtelter Plattenpacker:** `packPlatesNested()` rastert die echte Grundfläche im 2-mm-Raster, probiert vier Drehlagen je Teil und hält 2 mm Sicherheitsabstand (dilatiert). L-förmige Kurven und Tangenten verschachteln sich dadurch; die Beispiel-Auswahl (6× gerade 250, 1× 90°, 3× Tangente 90°, 4× Tangente 45° sowie die Hex-Skins) braucht 6 statt 8 Platten. Bambu Studios eigene Anordnung braucht 5 Platten, mischt aber Tracks und Skins.
- **Neutrale Druckwerte:** Die echten Profile bringen die Einstellungen ihrer Fremdprojekte mit. `NEUTRAL_OVERRIDES` + `profileFor()` setzen beim Export auf `0.20mm Standard`: `brim_type auto_brim` (statt `outer_only` mit 5 mm), `enable_support 0`, `curr_bed_type Textured PEI Plate` (statt High Temp), `layer_height 0.2` (statt 0.16 Optimal), 0.20-Schalen und -Geschwindigkeiten, `raft_first_layer_expansion -1`, `tree_support_wall_count -1`; `different_settings_to_system` wird geleert. Es werden **nur Werte** ersetzt, keine Key-Typen oder Array-Längen geändert.
- **A1-Version korrigiert:** `PRINTERS.a1.ver` schrieb `02.01.01.52` in `Application` und `slice_info.config`; jetzt `02.00.03.54` passend zum A1-Profil.
- **Hinweistext:** Der Modus „nicht drehen" nennt keine feste Drehachse mehr, sondern „größte Fläche nach unten" (die Achse hängt vom Teil ab).
- `check.js` prüft zusätzlich die neutralen Exportwerte, den unveränderten Key-Satz, `layLargest`/`faceAreaByAxis`/`rotZ` und den verschachtelten Packer.
- **Testdateien:** `test-p1s-0.3.4-alpha.3mf` und `test-a1-0.3.4-alpha.3mf` mit der Hex-Auswahl aus `duality-a1-hex_a12.3mf` (14 Track- + 7 Skin-Teile) – 21 Instanzen auf 6 Platten (4 Track + 2 Skin), nichts hochkant; per Python validiert. Die alten `test-*-0.3.3-alpha.3mf` entfallen.

### Neu (Druckerauswahl und Druckbarkeits-Filter)

- **Druckerwahl in allen Tabs:** Die Auswahl (`state.exp.printer`) steht jetzt auch in der Streifenliste und in der Wandplaner-Palette und ist mit dem 3MF-Export synchron. `PRINTER_IDS`/`FITS_IDS` spiegeln die Auswahl wie die bestehenden Filter (`setPrinter`/`setFitsOnly`).
- **Checkbox „nur druckbare Teile":** `fitsPrinter(file, key)` prüft gegen `PART_BBOX` (STL-Maße, neu im generierten Datenblock): flach = die zwei längsten Kanten passen auf die Platte, hochkant = die zwei kürzesten passen und die längste in die Bauhöhe. Ausgeblendete Teile werden unter der Liste gezählt (`fitsHidden`/`fitsNone`). Der Export zeigt weiterhin alle gewählten Teile und markiert nicht passende – nichts verschwindet stillschweigend.
- **Snapmaker U1:** neuer Eintrag mit 270 × 270 × 270 mm (Herstellerangabe) für den Filter, **bewusst ohne Export-Profil** – ein 3MF-Profil muss 1:1 aus einer echten, geprüft ladenden Projektdatei stammen. Der Export meldet „kein 3MF-Export-Profil"; die Plattenprüfung nutzt die 270-mm-Platte. Beispiel: 260-mm-Skins (Nautilus/Matte Hex/3D-Hex) sind auf dem P1S nicht druckbar, auf dem U1 schon.
- `check.js` prüft U1 (Maße, kein Profil, `profileFor('u1') === ''`), `PART_BBOX` für alle 56 Teile sowie `partDims`/`fitsPrinter` (250 passt, 400 nicht, 272 nicht auf U1, 260-mm-Skin nur auf U1).

## [0.3.3-alpha] – 2026-11-20

### Geändert (User-Feedback zum 3MF-Export)

- **Keine Baugruppen mehr:** `<assemble_item>` wird nicht mehr geschrieben (`<assemble>` bleibt leer). Tracks und Skins liegen auf **getrennten Platten**, innerhalb jeder Gruppe wird der Regalpacker möglichst dicht gepackt. So lassen sich Tracks in einer Farbe und Skins in zwei Farben zuweisen (Diffusionsschicht weiß, Farbenwechsel laut `guide.pdf` bei 0,5 mm).
- **Neue Ausrichtungs-Option:** „automatisch flach legen" dreht jedes Teil um die Y-Achse um +90° – genau wie in der echten Bambu-Vorlage des Designers (die STLs liegen mit der Länge auf Z und der 35,8-mm-Breite auf X; danach liegt die Länge flach in X). „nicht drehen, Hinweis anzeigen" behält die Ausrichtung und listet die Teile, die in Bambu Studio gedreht werden sollten.
- **Hinweise:** Track-Teile ohne Skin im gewählten Muster werden genannt (z. B. Tangenten bei den Original-Mustern – passende Skins gibt es nur bei Nautilus/Matte Hex/3D-Hex). **Zu lange Teile werden hochkant exportiert statt übersprungen** (User-Entscheidung 20.11.2026): Die 260-mm-Nautilus-Skins passen flach nicht auf 256 mm, bleiben aber in der STL-Ausrichtung erhalten und werden mit `expUpright` gemeldet; in Bambu Studio dann prüfen/anpassen.
- **A1-Profil:** `project_settings.config` wird **1:1 aus einer echten, im Bambu Studio des Users ladenden A1-3MF** übernommen (`/mnt/Druck/Gardena Adapter/Gardena_AG_a1.3mf`, `02.00.03.54`, 437 Keys, `printer_model: Bambu Lab A1`, `printer_structure: i3`). Drei Versuche, die Config aus dem neueren 02.09-Profil und der P1S-Vorlage zusammenzusetzen (Key-Satz/Versionsnummer angleichen, dann `top_solid_infill_flow_ratio` Array→Skalar), wurden vom Slicer weiterhin abgelehnt („Invalid configuration" + „keine Geometriedaten"). Erst die echte Config lädt – vom User mit `test-a1-realconfig.3mf` bestätigt; die finale `test-a1-0.3.3-alpha.3mf` ist byte-gleich dazu. `compatProfile()` und die Typ-Gleichheits-Checks sind entfallen; `check.js` prüft A1-Kennung, Struktur und Vollständigkeit (≥ 400 Keys). Eine weitere echte A1-3MF (`A1-Beispiel-Projekt.3mf`, `02.09.01.70`, 638 Keys) liegt als Referenz vor, wird aber bewusst **nicht** übernommen (gleiches 02.09-Muster wie die fehlgeschlagene Variante; User-Entscheidung 20.11.2026).

## [0.3.2-alpha] – 2026-11-20

### Behoben (3MF-Öffnen in Bambu Studio)

- `Metadata/model_settings.config` enthält jetzt das vollständige Bambu-Format: `face_count` je Objekt, `mesh_stat` und `source_*` je Part sowie `<assemble_item>`-Einträge mit der Platzierung pro Instanz. Die erzeugte 3MF sollte sich damit ohne „Invalid configuration“/„The file does not contain any geometry data“ öffnen lassen.
- `[Content_Types].xml` wird als erster ZIP-Eintrag geschrieben (OPC-Konvention).
- User-Test: beim **P1S** sind beide Fehler weg; beim **A1** bestehen sie weiterhin (siehe `0.3.3-alpha`).

## [0.3.1-alpha] – 2026-11-20

### Behoben (JSON-Bild und STL-Import)

- JSON-Export sichert jetzt auch das Wandplan-PNG (Base64); nach dem Import erscheint es im Wandplaner als Vorschau und lässt sich erneut herunterladen. Der Import zeichnet den Plan neu und stellt den gespeicherten Tab wieder her.
- STL-Import in Brave: Neben der Ordnerauswahl gibt es jetzt eine Schaltfläche für einzelne STL-Dateien. Drag & Drop erkennt Ordner über `FileSystemHandle` **und** die webkit-Entries; ohne gefundene STLs erscheint ein klarer Hinweis statt „0 STL-Dateien geladen".

## [0.3.0-alpha] – 2026-11-20

### Hinzugefügt (3MF-Export)

- Dritter Tab „3MF-Export": lokale STLs per Ordner-Picker oder Drag & Drop laden – die Dateien bleiben im Browser, nichts wird hochgeladen
- Musterwahl (Hex, Blank, Bars, Kabelmanagement, Nautilus, Matte Hex, 3D-Hex) leitet aus Stückliste oder Wandplan die passenden Skin-STLs ab; Stückzahlen sind in einer Tabelle editierbar
- Druckerwahl P1S oder A1 mit vollständigem Bambu-Studio-Profil; automatische Plattenaufteilung (Regalpacker) und Download einer `.3MF` mit einem Tab je Platte
- Der Export übernimmt die Export-Ausrichtung aus den STLs ohne Drehen; automatische Drehung bleibt TODO, zu hohe Teile werden hingewiesen

### Technik

- Die Musterzuordnung wird aus den echten Dateinamen der lokalen STLs erzeugt (`build_export_data.py`), die Bambu-Profile stammen aus echten Studio-Exporten (P1S aus dem lokalen 3MF, A1 aus dem Vault-Print-Planner-Profil, PLA-Variante)
- `check.js` prüft zusätzlich CRC32, STL-Parser, Packer, Stücklistenableitung und die erzeugte 3MF-Struktur (DOM-frei, Marker `// <export>`)

## [0.2.0-alpha] – 2026-10-09

### Hinzugefügt (Wandplaner und Filter)

- Wandplaner als zweiter Tab: Teile per Drag & Drop auf eine Wand, maßstäbliche Bänder (35,8 mm), Wandmaße und 50-mm-Raster
- Magnet-Snapping an freien Endpunkten (45°-Raster), `Shift` schaltet den Magnet ab
- Drehen (R/Shift+R), Spiegeln (F), Löschen (Entf), Rückgängig (Strg+Z), Zoom/Pan, Inspector, „Plan als Stückliste"
- Drehen im Inspector in beide Richtungen (+45° und −45°) – auch für Tablets ohne Tastatur
- Klonen des gewählten Teils: wird am freien Ende angesetzt und läuft tangential weiter (bei Geraden identische Ausrichtung; Knopf im Inspector oder Strg+D)
- Wandplan als PNG exportieren („Bild speichern", auf die Wand zugeschnitten)
- Wandplan in localStorage und JSON-Export/-Import; der Export markiert die App-Version
- Filter nach Serie (Original/Evolution) und Form (gerade/gebogen) – gemeinsam für Streifenliste und Wandplaner-Palette
- Dark Mode mit Umschalter (Sonne/Mond) in der Kopfzeile; gespeichert, Standard folgt der System-Einstellung

### Geändert

- Links (Bundle, Einzelprojekte, Designer) als eigener Abschnitt unten auf der Seite; die früheren Kopfzeilen-Links sind entfallen
- Benutzeroberfläche zweisprachig (Deutsch/English, Umschalter oben rechts); Längen-Badges als Nennwert/Rundung dargestellt

### Behoben (Wandplaner)

- Wandplaner-Tab zeigte die Streifenliste darüber (`hidden` wurde von `.grid{display:grid}` überstimmt)
- Ziehen aus der Palette war per Touch nicht möglich (fehlendes `touch-action:none`)

## [0.1.0] – 2026-10-08

### Hinzugefügt (Erstversion)

- Erste Version: Streifenrechner aller 56 strip-führenden Teile mit Pfadlänge, Auswahl, Summe/Rollen-Rest, JSON-Export/-Import
