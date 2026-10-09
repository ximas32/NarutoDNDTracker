# Kampf-Tracker — Spezifikation

Eine statische Website für GitHub Pages, die mir als Spielleiter während der Kämpfe meines Naruto-D&D-Homebrews hilft. Sie verwaltet Initiative-Reihenfolge, HP, Chakra und Zustände.

---

## Rahmenbedingungen

**Technisch**
- Läuft auf **GitHub Pages** → rein statisch, kein Server, kein Build-Schritt nötig
- **Reines HTML + CSS + JavaScript**, am liebsten eine einzige `index.html` mit allem drin
- Keine externen Abhängigkeiten, die beim Laden zwingend erreichbar sein müssen — die Seite muss funktionieren, auch wenn im Spielraum das WLAN schlecht ist
- Speicherung über **localStorage** (kein Login, keine Datenbank)

**Nutzungskontext**
- Ein einziger Nutzer: der Spielleiter
- Bedient wird es **während des Spiels**, oft einhändig, auf Handy oder Laptop
- 6 Spieler am Tisch, im größten Kampf bis zu 9 Gegner gleichzeitig
- Die Spieler dürfen den Bildschirm **nicht** sehen — Gegnerwerte bleiben verdeckt

**Designprinzip**
> Während des Kampfes soll **keine Tastatureingabe** nötig sein. Alles über große, gut treffbare Schaltflächen. Jede Sekunde, die ich auf den Bildschirm schaue, fehlt am Tisch.

---

## Funktion 1 — Szene vorbereiten

Vor dem Spielabend lege ich Kämpfe an und speichere sie.

**Pro Gegner erfasse ich:**

| Feld | Typ | Pflicht | Bemerkung |
|---|---|---|---|
| Name | Text | ja | z. B. „Söldner-Hauptmann" |
| AC | Zahl | ja | Rüstungsklasse |
| HP (max) | Zahl | ja | aktuelle HP starten auf diesem Wert |
| Chakra (max) | Zahl | nein | leer lassen, wenn der Gegner keins hat |
| Initiative-Bonus | Zahl | ja | z. B. `+5` |
| Anzahl | Zahl | ja | legt mehrere identische Gegner an (siehe unten) |

**Mehrfach-Gegner:** Gebe ich bei „Anzahl" eine 4 ein, entstehen vier eigenständige Einträge mit eigenen HP-Balken, automatisch nummeriert: *Kleine Puppe 1*, *Kleine Puppe 2* usw.

**Ergänzungen, die ich zusätzlich brauche:**

- **Notizfeld pro Gegner** (eine Zeile, frei): Für Dinge wie „Kampfrausch ab ≤27 HP" oder „Angriff +7, 1W8+4". Wird in der Kampfansicht klein unter dem Namen angezeigt, damit ich die Angriffswerte nicht im Heft suchen muss.
- **Schwellenwert mit Hinweis** (optional pro Gegner): Ein HP-Wert plus Text. Sinkt der Gegner auf oder unter diesen Wert, hebt die App ihn sichtbar hervor und zeigt den Text. Beispiel: Schwelle `27`, Text „Kampfrausch: +2 Angriff, zweiter Angriff". Das ist die Funktion, die mir am Tisch am meisten Fehler erspart.
- **Szene benennen und speichern**, z. B. „Hinterhalt an der Brücke" und „Endkampf Kazen". Beim nächsten Mal lade ich die Szene und alles steht wieder bereit.
- **Szenen exportieren und importieren als JSON-Datei.** Wichtig, damit meine Vorbereitung nicht verloren geht, wenn ich den Browser-Speicher lösche oder das Gerät wechsle.

---

## Funktion 2 — Spielerliste

Die Spieler verwalte ich getrennt von den Gegnern, weil sie über alle Szenen gleich bleiben.

- **Nur Name und Initiative-Wert** nötig
- Die Liste bleibt dauerhaft gespeichert und wird bei jeder neuen Szene wiederverwendet
- HP und Chakra der Spieler werden **nicht** getrackt — das machen die Spieler selbst auf ihren Blättern
- Ein Spieler lässt sich für eine Szene **deaktivieren**, falls jemand fehlt

---

## Funktion 3 — Kampf durchführen

Das Herzstück. Ablauf:

### Initiative festlegen
- Spieler und Gegner erscheinen gemeinsam in einer Liste
- Für Gegner: **Button „Initiative würfeln"**, der automatisch W20 + Bonus wirft (für alle Gegner auf einmal, gleiche Gegnertypen dürfen denselben Wert teilen)
- Spielerwerte kommen aus der Spielerliste, sind aber **manuell überschreibbar**
- Die Reihenfolge lässt sich danach **per Drag-and-drop korrigieren** — ich will das letzte Wort haben

### Kampfansicht
Eine durchgehende, von oben nach unten lesbare Liste in Initiative-Reihenfolge. Pro Eintrag sichtbar:

- Name und Initiative-Wert
- Bei Gegnern: **aktuelle HP / max HP** als Zahl *und* als Balken, AC, ggf. Chakra
- Bei Spielern: nur der Name (keine Werte)
- Das Notizfeld, klein
- Aktive Zustände als farbige Marker

**Der aktuell handelnde Charakter ist deutlich hervorgehoben** — groß, farbig, unübersehbar, damit ich ihn aus zwei Metern Entfernung erkenne.

### Schaden und Chakra eingeben
- Beim Antippen eines Gegners öffnet sich ein Eingabefeld mit **Zifferntasten auf dem Bildschirm** (kein Betriebssystem-Keyboard)
- **Schaden abziehen** und **Heilung addieren** als getrennte Schaltflächen
- **Chakra abziehen** separat
- Schnellwahl-Buttons für häufige Werte wären hilfreich: −1, −3, −5, −10
- Fällt ein Gegner auf 0 HP, wird er **ausgegraut und ans Ende sortiert**, aber nicht gelöscht

### Rundensteuerung
- Button **„Nächster"** schaltet zum nächsten Charakter in der Reihenfolge
- Nach dem Letzten beginnt automatisch eine neue Runde, ein **Rundenzähler** erhöht sich
- Der Rundenzähler ist wichtig, weil viele Effekte bei mir „2 Runden" dauern

---

## Ergänzungen, die ich für nötig halte

Diese standen nicht in meiner ursprünglichen Liste, ergeben sich aber aus meinem Regelsystem.

### Zustände mit Rundenzähler
Mein System kennt vier Zustände: **vergiftet, Chakra-Starre, liegend, festgesetzt**. Dazu kommen temporäre Modifikatoren aus Genjutsu („−2 auf Angriffe, 2 Runden").

- Pro Charakter (auch Spieler!) lassen sich Zustände per Antippen setzen
- Jeder Zustand bekommt optional eine **Restdauer in Runden**, die beim Rundenwechsel automatisch herunterzählt
- Läuft ein Zustand ab, zeigt die App einen deutlichen Hinweis
- Bei Spielern will ich Zustände tracken können, obwohl ich ihre HP nicht tracke — das vergesse ich am Tisch sonst ständig

### Rückgängig machen
Ein **Undo für die letzte Aktion**. Ich vertippe mich sicher beim Schadenabziehen, und ohne Undo muss ich rechnen.

### Gegner während des Kampfes hinzufügen
In meinem Endkampf ruft der Boss unterwegs Verstärkung. Ich brauche einen Button **„Gegner hinzufügen"**, der mitten im Kampf einen vorbereiteten Gegner aus einer Reserve-Liste einfügt und ihn direkt in die Initiative einsortiert.

### Gefallene Gegner zurückholen
Spezialfall aus meinem Endkampf: Ein Gegner kann gefallene Verbündete mit halben HP wieder aufstellen. Ein Button an jedem gefallenen Gegner — **„zurückholen (halbe HP)"** — würde das abdecken.

### Dunkles Design
Wir spielen abends. Ein **dunkles Farbschema** als Standard ist angenehmer und blendet die Spieler nicht.

### Bildschirm wach halten
Wenn möglich: verhindern, dass sich das Handy während des Kampfes sperrt (Wake Lock). Falls das zu kompliziert ist, weglassen — kein Muss.

### Kampf zurücksetzen
Ein Button, der alle HP, Chakra, Zustände und den Rundenzähler auf den Ausgangszustand zurücksetzt, ohne die Szenen-Vorbereitung zu löschen. Für den Fall, dass wir einen Kampf wiederholen oder ich etwas verprobiert habe.

---

## Was die Seite ausdrücklich **nicht** können muss

- Keine Benutzerkonten, kein Mehrspieler-Betrieb, keine Synchronisation zwischen Geräten
- Keine Würfelfunktion außer dem Initiative-Wurf — wir würfeln am Tisch mit echten Würfeln
- Keine Regel-Automatik: Die App rechnet keinen Schaden aus und prüft keine Treffer, sie zählt nur mit
- Keine Spieler-HP-Verwaltung
- Keine Charakterverwaltung

---

## Vorbereitete Gegnerdaten

Diese Gegner hätte ich gern direkt eingebaut, sodass ich sie nicht abtippen muss. Sie stammen aus meiner fertigen Episode.

### Szene „Hinterhalt an der Brücke"

| Name | Anzahl | AC | HP | Chakra | Init | Notiz |
|---|---|---|---|---|---|---|
| Söldner | 6 | 13 | 22 | — | +1 | Angriff +4, 1W8+2. Ergibt sich bei halben Verlusten |
| Söldner-Hauptmann | 1 | 15 | 55 | — | +3 | Angriff +6, 1W10+3. Erdstoss 1×: STR-SG 13, 1W8 + liegend |

Schwellenwert Hauptmann: **27 HP** → „Kampfrausch: +2 Angriff und zweiter Nahkampfangriff pro Runde"

### Szene „Endkampf — Phase 1"

| Name | Anzahl | AC | HP | Chakra | Init | Notiz |
|---|---|---|---|---|---|---|
| Marionette Yanis | 1 | 15 | 60 | — | +5 | 2 Angriffe +7, je 1W8+4. Genjutsu ohne Save. Bewegung 15 m |
| Kleine Puppe | 4 | 13 | 12 | — | +2 | Angriff +5, 1W6+2. Immun gegen Gift und Genjutsu |
| Kazen | 1 | 16 | 90 | 40 | +3 | **Phase 1: greift nicht an, ist unerreichbar.** Angriff +7, SG 14 |

Hinweis für die App: Yanis holt zu Beginn seines Zuges **eine gefallene kleine Puppe mit 6 HP** zurück → dafür ist der Button „zurückholen (halbe HP)" gedacht.

### Szene „Endkampf — Phase 2" (Verstärkung, wird nachgerufen)

| Name | Anzahl | AC | HP | Chakra | Init | Notiz |
|---|---|---|---|---|---|---|
| Feuerspucker-Puppe | 1 | 14 | 25 | — | +2 | Kegel 4,5 m, DEX-SG 13, 2W6 Feuer |
| Windstoss-Puppe | 1 | 14 | 25 | — | +2 | Linie 12 m, STR-SG 13, 1W8 + 3 m zurück + liegend |
| Gift-Puppe | 1 | 14 | 25 | — | +2 | Angriff +6, 1W6+2, CON-SG 13 → vergiftet |
| Blitz-Puppe | 1 | 14 | 25 | — | +2 | Reserve. Angriff +6, 1W6+2, Ziel verliert Reaktion |
| Erd-Puppe | 1 | 14 | 25 | — | +2 | Reserve. Angriff +6, 1W8+3 |
| Wasser-Puppe | 1 | 14 | 25 | — | +2 | Reserve. Angriff +6, 1W6+2, Ziel 3 m ziehen |

### Weitere Gegner

| Name | AC | HP | Chakra | Init | Notiz |
|---|---|---|---|---|---|
| Suna-Patrouillen-Ninja | 15 | 34 | — | +3 | Angriff +5, 1W8+2. Nimmt gefangen statt zu töten |

---

## Priorisierung

Falls nicht alles auf einmal geht, in dieser Reihenfolge:

1. Szene anlegen mit Gegnern, Initiative-Liste, HP-Tracking, Rundenzähler
2. Speichern im localStorage und Szenen laden
3. Zustände mit Rundenzähler
4. Undo, Gegner nachträglich hinzufügen, gefallene zurückholen
5. JSON-Export/Import, Schwellenwert-Hinweise
6. Dunkles Design, Wake Lock

Punkt 1 und 2 allein würden mir am Spieltisch schon sehr helfen.
