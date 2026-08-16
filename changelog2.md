Alle wichtigen Änderungen und Entwicklungsschritte der Autoren-Suite.

Die Autoren-Suite wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt. Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

----

## v1.0.4 – Inspiration UX

### 🖼️ Inspiration UX

- Ein normaler Linksklick auf ein Inspirationsbild öffnet jetzt eine große, modern abgedunkelte Lightbox; Rechtsklick bleibt für Aktionen reserviert.
- In der Lightbox kann mit Pfeiltasten bzw. Vor/Zurück-Schaltern durch die aktuell sichtbaren Bilder geblättert werden.
- Inspirationsbilder lassen sich mit dem vorhandenen Drag-Griff jetzt direkt auf Kategorien ziehen; „Alle Inspirationen“ entfernt die Kategoriezuordnung wieder.
- Das Rechtsklick-Menü wurde um Vergrößern und „In Kategorie verschieben“ ergänzt.
- „In Büchern verwenden“ ist im Bearbeiten-Dialog standardmäßig eingeklappt, zeigt kompakt die Zahl vorhandener Zuordnungen und besitzt nach dem Aufklappen eine Buchsuche sowie gruppierte Buchlisten.
- „Alle Inspirationen“ sortiert standardmäßig nach letzter echter Bearbeitung: neue und bearbeitete Medien stehen oben; reines Öffnen verändert die Reihenfolge nicht.
- Veraltete grüne Kartenrahmen wurden entfernt und Fokus-/Drop-Zustände an den ruhigeren Suite-Stil angepasst.

### ✓ Technische Prüfung

- Keine Datenbankmigration erforderlich.
- Bestehende Inspirationen, Ordner, Buchmedien-Zuordnungen und manuelle Ordnersortierungen bleiben kompatibel.

## v1.0.3

### ✒️ Manuskript & Fokus

- Der Manuskript-Arbeitsplatz nutzt die verfügbare Fensterhöhe jetzt deutlich konsequenter; die bisherige feste Obergrenze von 920 px entfällt.
- Freier Platz unterhalb des Editors wird dem eigentlichen Schreibfeld zugeschlagen, statt als ungenutzte Fläche stehenzubleiben.
- Die Breite des Manuskripts wurde dabei bewusst nicht vergrößert.
- Der obere Schreibkopf wurde weiter beruhigt: häufige Aktionen bleiben direkt sichtbar, seltenere Schreib- und Buchwerkzeuge liegen in kompakten Dropdown-Menüs.
- Undo/Redo bleiben als kleine Direktaktionen erreichbar.

### 🎯 Fokusmodus

- Der Fokusmodus nutzt nahezu die gesamte verfügbare Viewport-Höhe für den Schreibbereich.
- Workflow-Leisten, Navigator und Inspector bleiben dort ausgeblendet.
- Sichtbar bleiben nur der minimale Buchkontext, Schreibsession/Timer und der Schalter zum Beenden des Fokusmodus.
- Editorüberschrift, Toolbar und Statusleiste wurden im Fokusmodus vertikal verdichtet.

### ◀ Seitenleisten

- Neben dem Navigator kann jetzt auch der rechte Inspector direkt im Manuskript ein- und ausgeblendet werden.
- Ein dezenter Wiederherstellungs-Schalter bringt den Inspector zurück.
- Navigator und Inspector lassen sich unabhängig voneinander ausblenden; beide Zustände werden lokal gemerkt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine Datenbankmigration erforderlich.
- Keine Manuskript-, Story- oder Versionsdaten verändert.

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
