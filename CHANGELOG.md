# Changelog

Versionierung: SemVer mit Kanal-Suffix (`-alpha`, `-beta`, `-rc`); der Kanal wird in der App angezeigt.
Die oberste Version hier muss der Konstante `APP_VERSION` in `duality-streifenrechner.html` entsprechen (prüft `check.js`).
Derselbe Changelog ist in der App unter „Änderungen" eingeblendet.

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
