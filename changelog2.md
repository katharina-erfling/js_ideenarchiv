Alle wichtigen Änderungen und Entwicklungsschritte der Autoren-Suite.

Die Autoren-Suite wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt. Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

----
## v1.0.9

### 💾 Backup klarer als vollständige Suite-Sicherung

- Die Backup-Aktionen heißen in der Oberfläche jetzt ausdrücklich `Suite-Backup` bzw. `Vollständiges Autoren-Suite-Backup`.
- Datensicherheit erklärt direkt, dass ein vollständiges Backup nicht nur Ideenzettel, sondern auch Bücher/Reihen, Manuskripte, Story-Bibel, Timeline, Versionen/Revision, Medien und wichtige Einstellungen sichert.
- Auch das Exportzentrum zeigt den Umfang der vollständigen Sicherung als kompakte Inhaltsübersicht.
- Der Unterschied zwischen vollständiger Wiederherstellung und reinem Ideen-Zusammenführen wird klarer beschrieben: Ohne Ersetzen werden bewusst nur Ideen, Kategorien und Schnellregister ergänzt.
- Exportformate wie PDF, EPUB, DOCX, RTF, HTML, Markdown und TXT werden weiterhin ausdrücklich nicht als Ersatz für ein vollständiges Backup dargestellt.

### 🕰️ Heute im Schreibzimmer

- Die bisher verkürzte Bezeichnung `Zettel` wurde in der Tageszusammenfassung zu `Ideenzettel` geändert, damit eindeutig ist, was gezählt wird.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine Datenbankmigration erforderlich.
- Keine Ideen-, Buch-, Manuskript-, Story- oder Backupdaten strukturell verändert.

## v1.0.8

### 🖱️ Interaktions-Konsistenz

- Recherchekarten besitzen jetzt ein eigenes Rechtsklick-Menü mit Details, Bearbeiten, Quelle öffnen, Verschieben in Sammlungen und sicherem Zugang zum Löschen.
- Recherchesammlungen reagieren ebenfalls auf Rechtsklick und bieten Öffnen, neuen Eintrag, Untersammlung und Bearbeiten direkt am Objekt an.
- Inspirationskategorien besitzen ergänzend ein passendes Kontextmenü für Öffnen, Unterkategorie und Bearbeiten; Linksklick auf Inspirationsmedien bleibt weiterhin für die Lightbox reserviert.
- Die neuen Kontextmenüs verwenden einen gemeinsamen viewport-sicheren Stil und bleiben auch bei langen Sammlungslisten scrollbar.

### ↶ Rückgängig bei Verschieben

- Das Verschieben von Inspirationen zwischen Kategorien bietet jetzt unmittelbar `Rückgängig`, statt für eine ungefährliche Ordnungsaktion zusätzliche Bestätigungsdialoge zu benötigen.
- Dasselbe gilt beim Verschieben von Rechercheeinträgen zwischen Recherchesammlungen.
- Kritische Aktionen wie echtes Löschen bleiben weiterhin bewusst abgesichert.

### ◇ Leere Filterzustände

- Recherche und Inspiration unterscheiden jetzt klar zwischen `noch keine Inhalte` und `der aktuelle Filter findet nichts`.
- Bei leeren Such-/Filterergebnissen erscheint ein eigener verständlicher Zustand mit direkter Aktion `Filter zurücksetzen`.
- Vorhandene Inhalte werden dadurch nicht mehr wie ein tatsächlich leeres Archiv dargestellt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine Datenbankmigration erforderlich.
- Keine Story-, Manuskript-, Recherche- oder Inspirationsdaten strukturell verändert.

## v1.0.7

### 📊 Dashboards & Statistiken

- Das allgemeine Dashboard zeigt jetzt einen kompakten 14-Tage-Aktivitätsverlauf für Schreiben und neue Ideen statt einer weiteren reinen Zahlenkarte.
- Das Buch-Dashboard erhält zwei visuelle Schnellübersichten für Scene Lifecycle und Schreibverlauf der letzten 14 Tage; beide führen direkt in den passenden Arbeitsbereich.
- Das Ideenarchiv-Dashboard ergänzt die Kategorienansicht um eine visuelle Verteilung, während die bestehenden anklickbaren Kategorien erhalten bleiben.
- Die Reihenübersicht zeigt den Entwicklungsstand ihrer Bände jetzt zusätzlich als kompakte, anklickbare Fortschrittsgrafik.
- Die Statistikansicht besitzt einen neuen Bereich `Visuelle Übersicht` mit Lifecycle-Verteilung, Schreibaktivität nach Wochentag und Entwicklungsstand der Bücher im aktuellen Filter.
- Diagramme verwenden ausschließlich bereits vorhandene Story-, Schreib- und Ideendaten und erzeugen keine parallelen Statistikdaten.
- Wo ein Diagramm ein konkretes Objekt repräsentiert, führt ein Klick direkt zum betreffenden Buch bzw. Arbeitsbereich.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine Datenbankmigration erforderlich.
- Keine Manuskript-, Story-, Ideen- oder Statistikdaten verändert.

## v1.0.6

### 📚 Bücher & Reihen Polish

- Der Zugang `Reihenübersicht` im Bücherbereich wurde optisch beruhigt und fügt sich als neutraler Werkzeugbutton in die Bibliotheksleiste ein.
- Die Reihenübersicht öffnet jetzt als schwebendes, scrollbar begrenztes Panel statt den gesamten Bücherregal-Inhalt nach unten zu schieben.
- Die Übersicht kann über `×` oder `Esc` geschlossen werden; beim Öffnen eines Bandes schließt sie sich automatisch.
- Kleine Beschriftungen in Reihen-, Band-, Kennzahlen- und Cross-Book-Bereichen wurden deutlich lesbarer gesetzt.
- Karten und Kennzahlen wurden kompakter angeordnet, sodass trotz größerer Schrift mehr Informationen ohne unnötige vertikale Länge sichtbar bleiben.
- Auf kleineren Displays passt sich das Panel an den verfügbaren Viewport an und bleibt intern scrollbar.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine Datenbankmigration erforderlich.
- Keine Bücher-, Reihen-, Story-Bibel- oder Living-Canon-Daten verändert.

## v1.0.5

### 💡 Ideenarchiv-Dashboard

- Beim Öffnen des Ideenarchivs erscheint jetzt zuerst ein eigenes Ideen-Dashboard statt unmittelbar der vollständigen Zettelwand.
- Das Dashboard zeigt Ideenzettel gesamt, heutige Aktivität, Favoriten, noch keinem Buch zugeordnete Ideen und Romanideen.
- Ein 14-Tage-Verlauf macht das Ideenwachstum sichtbar, ohne daraus Produktivitätsdruck abzuleiten.
- Häufig genutzte Kategorien, zuletzt bearbeitete Ideen und ein Bereich `Wiederentdecken` bringen ältere Gedanken zurück ins Blickfeld.
- Schnellaktionen führen direkt zu neuer Idee, allen Ideen oder zum vorhandenen Zufalls-/Wiederentdecken-Werkzeug.

### 🏠 Allgemeines Dashboard

- Das Ideenarchiv ist auf dem allgemeinen Autoren-Suite-Dashboard wieder stärker präsent.
- Der Ideenblock zeigt kompakt heutige Ideenaktivität, Favoriten, Romanideen und die zuletzt bearbeiteten Ideenzettel.
- Von dort führt ein eigener Link direkt ins neue Ideen-Dashboard.

### 🗒️ Lange Ideenzettel

- Ideentexte bleiben inhaltlich unbegrenzt lang.
- In der normalen Karten-/Listenansicht besitzt der Textbereich nun eine sinnvolle Maximalhöhe mit eigenem Scrollbereich.
- Eine einzelne sehr lange Idee kann dadurch nicht mehr die gesamte Zettelansicht auseinanderziehen.
- In Detail- und Fokusansichten bleibt der vollständige Inhalt weiterhin komfortabel zugänglich.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine Datenbankmigration erforderlich.
- Bestehende Ideen, Kategorien, Favoriten und Buchverknüpfungen bleiben unverändert kompatibel.

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
