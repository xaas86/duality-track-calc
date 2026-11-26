# Changelog

Öffentliche (aggregierte) Versionshistorie. Die interne Entwicklungshistorie
mit Alpha-Versionen liegt in `CHANGELOG.md` (privates Repo).

## [0.3] – 2026-11-26

### Neu
- **Streifenrechner:** Alle 56 strip-führenden Teile mit Pfadlänge, Auswahl, Summe/Rollen-Rest, JSON-Export/-Import.
- **Wandplaner:** Teile per Drag & Drop auf eine Wand, maßstäbliche Bänder, Magnet-Snapping, Drehen/Spiegeln, PNG-Export, „Plan als Stückliste".
- **3MF-Export:** Lokale STLs → downloadbare 3MF für Bambu Studio (P1S, A1) und Snapmaker Orca (U1) mit automatischer Plattenaufteilung.
- **Zubehör:** Splice-Blocks (automatisch Tracks − 1), End Mounts, Base Mounts, Light Couplings mit Stückzahlen.
- **Anleitung-Tab:** Inhalt der mitgelieferten Anleitung (Drucken, LED-Strips, 45°/90°-Tracks, Skins, Base Tracks, Montage, Installation).
- **UI:** Zweisprachig (Deutsch/English), Dark Mode, Hover-Vorschau, einklappbare Kategorien, Druckerwahl mit Druckbarkeits-Filter.

### Sicherheit
- DOM-XSS in Datei-/Ordnernamen im Export-Tab behoben, Content-Security-Policy ergänzt.

## [0.2] – 2026-10-09

### Neu
- Wandplaner als zweiter Tab (Drag & Drop, Magnet-Snapping, Drehen/Spiegeln, Zoom/Pan).
- Filter nach Serie (Original/Evolution) und Form (gerade/gebogen), gemeinsam für Liste und Palette.
- Dark Mode mit Umschalter, zweisprachige Oberfläche (DE/EN).

## [0.1] – 2026-10-08

### Erste Version
- Streifenrechner aller strip-führenden Teile mit Pfadlänge, Auswahl, Summe/Rollen-Rest, JSON-Export/-Import.