Alle wichtigen Änderungen und Entwicklungsschritte der Autoren-Suite.

Die Autoren-Suite wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt. Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

## v1.0.2

### ◀ Manuskript-Navigator ein-/ausblenden

- Der linke Manuskript-Navigator kann jetzt direkt im Schreibbereich über einen kleinen Pfeil eingeklappt werden.
- Das Manuskript nutzt den frei gewordenen Platz sofort; der rechte Inspector bleibt dabei erhalten.
- Ein dezenter `› Navigator`-Schalter am linken Rand der Schreibfläche blendet die Seitenleiste wieder ein.
- Der Zustand wird lokal gespeichert und bleibt beim erneuten Öffnen der Suite erhalten.
- Die bestehende Workspace-Studio-Einstellung für Navigator und Inspector bleibt kompatibel.

### ✓ Technische Prüfung

- Keine Datenbankmigration erforderlich.
- Keine Manuskript-, Story- oder Versionsdaten verändert.

## v1.0.1

### ✒️ Schreibbereich – mehr Platz für das Manuskript

- Der eigentliche Schreibarbeitsplatz nutzt auf Desktop-Bildschirmen deutlich mehr von der verfügbaren Fensterhöhe.
- Navigator, Manuskript und Inspector werden gemeinsam höher, ohne ihre interne Scrolllogik zu verändern.
- Die bisherige starre Höhenbegrenzung wurde für große Desktop-Viewports erweitert.

### ▤ Kompakterer Schreibkopf

- Projekt- und Buchauswahl stehen auf breiten Bildschirmen platzsparend nebeneinander statt untereinander.
- Die zusätzliche `SCHREIBPROGRAMM`-Kickerzeile wird im Desktop-Schreibkopf ausgeblendet, da der Kontext bereits eindeutig ist.
- Wortstand und Zielanzeige wurden kompakter angeordnet.
- Die Werkzeugbuttons im oberen Schreibkopf sind niedriger, enger gruppiert und nutzen kleinere Abstände, ohne Funktionen zu entfernen.
- Die vorhandenen Aktionen bleiben vollständig erreichbar; es wurde keine Schreib-, Versions-, Import-, Analyse- oder Sessionfunktion entfernt.

### 🧭 Navigation

- Die Workflow- und Manuskript-Unterleisten beanspruchen etwas weniger vertikale Höhe.
- Mobile und schmale Layouts behalten ihre bisherigen responsiven Umbruchregeln.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenbankmigration erforderlich.
- Keine Story-, Manuskript- oder Versionsdaten verändert.
