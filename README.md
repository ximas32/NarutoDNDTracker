# Kampf-Tracker

Statischer Kampf-Tracker für mein Naruto-D&D-Homebrew. Verwaltet Initiative,
HP, Chakra, Zustände und den Rundenzähler während der Spielrunde.

**Live:** https://ximas32.github.io/NarutoDNDTracker/

## Technik

Eine einzige `index.html` — HTML, CSS und JavaScript in einer Datei, keine
externen Abhängigkeiten, kein Build-Schritt. Funktioniert offline, sobald die
Seite einmal geladen ist. Alle Daten liegen im `localStorage` des Browsers.

## Funktionen

- **Szenen** mit Gegnern (Name, AC, HP, Chakra, Init-Bonus, Anzahl, Notiz,
  Schwellenwert mit Hinweistext, Reserve-Flag)
- **Spielerliste**, dauerhaft gespeichert, pro Szene einzeln deaktivierbar
- **Initiative** per W20 + Bonus für alle Gegner auf einmal; gleiche Gegnertypen
  teilen sich einen Wert. Reihenfolge per Drag-and-drop oder Pfeiltasten
  korrigierbar
- **Kampfansicht** in Initiative-Reihenfolge, der aktuell handelnde Charakter
  ist gross hervorgehoben
- **Zifferntasten auf dem Bildschirm** für Schaden, Heilung und Chakra —
  kein Betriebssystem-Keyboard nötig. Schnellwahl −1/−3/−5/−10/−15/−20
- **Zustände** (vergiftet, Chakra-Starre, liegend, festgesetzt, frei
  definierbar) mit optionaler Restdauer, die beim Rundenwechsel herunterzählt
- **Undo** für die letzten 40 Aktionen
- **Gefallene Gegner** werden ausgegraut ans Ende sortiert, nicht gelöscht,
  und lassen sich mit halben HP zurückholen
- **Gegner mitten im Kampf hinzufügen**, wird automatisch einsortiert
- **JSON-Export und -Import** für Szenen und Spieler
- Dunkles Design, Wake Lock gegen den Bildschirmschoner

## Bedienung

| | |
|---|---|
| Gegnerzeile antippen | Zifferntasten für Schaden |
| `Nächster ▸` | nächster Charakter, am Ende neue Runde |
| Leertaste | wie `Nächster` (Desktop) |
| `↶` | letzte Aktion rückgängig |
| Escape | Dialog schliessen |

## Vorbereitete Daten

Die Szenen „Hinterhalt an der Brücke", „Endkampf — Phase 1", „Endkampf —
Phase 2" und „Weitere Gegner" sind fest eingebaut und werden beim ersten Start
automatisch angelegt. Über *Vorlagen laden* lassen sie sich jederzeit
wiederherstellen.

Die vollständige Spezifikation steht in
[Kampf-Tracker_Spezifikation.md](Kampf-Tracker_Spezifikation.md).

## Hinweis zur Sichtbarkeit

GitHub Pages ist öffentlich erreichbar. Die Gegnerwerte stehen damit im Netz.
Wer das nicht will, löscht den `PRESETS`-Block in `index.html` und lädt seine
Szenen beim ersten Start einmalig per JSON-Import.
