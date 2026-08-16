Alle wichtigen Änderungen und Entwicklungsschritte der Autoren-Suite.

Die Autoren-Suite wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt. Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

----
## v1.2.5 – Cache-sicherer Manuskript-UI-Fix

### 🧯 Ursache der scheinbar wirkungslosen UI-Fixes
- `styles.css` und `app.js` wurden bisher in jeder Version unter exakt derselben lokalen URL eingebunden.
- Bei lokal geöffneten Paketen konnte der Browser dadurch ältere Asset-Versionen aus dem Cache weiterverwenden, obwohl das neue Paket bereits geänderte Dateien enthielt.
- Die Assets werden ab v1.2.5 mit der App-Version im URL-Query geladen (`styles.css?v=1.2.5`, `app.js?v=1.2.5`), damit ein neues Paket garantiert seine eigenen CSS-/JS-Dateien lädt.

### ✒ Manuskript-Kopf & Schreibhöhe
- Der kompakte Manuskript-Kopf aus v1.2.4 bleibt erhalten und wird zusätzlich durch einen kleinen kritischen CSS-Fallback direkt in `index.html` abgesichert.
- Titel, Fassung, Status und Schnellaktionen teilen sich damit zuverlässig die kompakte Kopfzone.
- Toolbar und Statusleiste bleiben verdichtet; die freie Höhe steht dem Manuskript zur Verfügung.
- Die dynamische Viewport-Messung aus v1.2.4 bleibt aktiv.

### ✓ Technische Prüfung
- App-Version auf 1.2.5 angehoben.
- JavaScript-Syntax geprüft.
- HTML-Asset-Referenzen versioniert.
- Keine Datenbankmigration erforderlich.
- Keine Manuskript- oder Storydaten verändert.

## v1.2.4 – Verifizierter UI-Fix & Implementierungs-Audit

### ✒ Manuskript – strukturell kompakter statt nur CSS-Padding

- Titel, Metadaten, kreative Fassung, Reifegrad und Schnellaktionen teilen sich jetzt denselben kompakten Dokumentkopf.
- Die Fassung liegt nicht mehr als eigene zusätzliche Zeile unter dem Titel.
- Die Manuskripthöhe wird zur Laufzeit aus der tatsächlichen Position des Schreibbereichs und der real verfügbaren Viewport-Höhe berechnet.
- Die Höhe wird nach Resize, Panel-/Workflow-Änderungen und beim Rendern des Manuskripts neu gemessen.
- Formatleiste und Statusleiste wurden nochmals vertikal verdichtet, ohne Funktionen zu entfernen.
- Navigator, Inspector, Dokumentkopf und Toolbars verwenden eine konsistentere UI-Typografie.
- Dokumenttitel und Manuskript bleiben bewusst Serifenschrift.
- Im Fokusmodus verschwinden zusätzliche Metadaten und die verbleibenden Leisten werden weiter reduziert.

### 🔎 Audit früherer Umsetzungsbehauptungen

- v1.0.1, v1.0.3 und v1.2.2 hatten die Manuskripthöhe nur über feste CSS-Abzüge vergrößert.
- v1.2.4 ersetzt diese Schätzung durch eine echte Viewport-Messung.
- Die in v1.2.2 behauptete deutliche Verdichtung des Dokumentkopfs war im sichtbaren Layout nicht ausreichend und wurde deshalb strukturell neu umgesetzt.
- Die Inspirations-Lightbox war zwischenzeitlich durch eine Paket-/Markup-Regression wieder aus dem DOM verschwunden, obwohl sie zuvor als umgesetzt dokumentiert war.
- Der grüne Inspirations-Fokusrahmen war in v1.2.1 nicht vollständig beseitigt.
- Für die übrigen v1.0.2–v1.2.3-Hauptfunktionen wurden im aktuellen Code die zugehörigen DOM-Strukturen/Funktionen statisch nachgewiesen.
- Visuelle Laufzeitkorrektheit wird künftig nicht allein aufgrund vorhandenen Codes als erledigt behauptet.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Interaktive Controls statisch auf fehlende Code-Referenzen geprüft.
- Keine IndexedDB-Migration erforderlich.
- Keine Manuskript- oder Storydaten strukturell verändert.

## v1.2.3 – Dashboard-Korrekturen & Inspiration-Performance

### 💡 Dashboard & Ideenarchiv
- Auf dem allgemeinen Dashboard zeigt der Ideenblock jetzt `heute gesammelt`, `Ideen insgesamt` und `Romanideen`; die dort nicht benötigte Favoritenzahl entfällt.
- Unter dem 14-Tage-Aktivitätsdiagramm wird neben den geschriebenen Wörtern jetzt auch die Zahl neuer Ideenzettel der letzten 7 Tage angezeigt.
- Das Ideen-Dashboard ersetzt `Favoriten` und `noch keinem Buch zugeordnet` durch `diesen Monat` und `Kategorien`.
- Der Kategorien-Donut ist visuell mit der Rangliste verbunden: Farben erscheinen sowohl im Ring als auch an Markern und Balken der jeweiligen Kategorie.
- Die Donut-Mitte ist transparent und zeigt damit exakt den Hintergrund der Dashboard-Karte; Zahl und Beschriftung bleiben sauber innerhalb des Rings.

### ✒ Zuletzt am Schreibtisch
- `Weiter schreiben` und der zuletzt bearbeitete Manuskriptabschnitt öffnen jetzt ausdrücklich den Manuskript-Reiter des richtigen Buchs und den gespeicherten Abschnitt, statt auf einen anderen Workflow-Bereich zu fallen.

### 🖼 Inspiration – Start, Performance & Lightbox
- Inspiration startet jetzt auf einem leichten eigenen Dashboard statt sofort das vollständige Medienboard zu rendern.
- Das Dashboard zeigt Bestand, Aktivität, Sammlungen, Buchverwendung, zuletzt bearbeitete Medien und ältere Inspirationen zum Wiederentdecken.
- `Alle Inspirationen` oder eine konkrete Sammlung öffnet erst auf Wunsch das vollständige Board.
- Sammlungszähler werden beim Rendern jetzt über eine einmal berechnete Hierarchie ausgewertet statt für jeden Ordner wiederholt das gesamte Archiv zu durchsuchen.
- Board-Bilder verwenden Lazy Loading und asynchrones Decoding, um unnötige Bilddekodierung beim Öffnen großer Ansichten zu reduzieren.
- Die seit v1.0.4 vorgesehene Lightbox ist jetzt tatsächlich im DOM vorhanden; Linksklick auf ein Bild öffnet sie zuverlässig.
- Lightbox schließen über `ESC`, `×` oder Klick auf den abgedunkelten Hintergrund; Pfeiltasten blättern durch die sichtbaren Bilder.
- Der hartnäckige grüne Fokus-/Auswahlrahmen bei Inspirationskarten wurde an der Fokusursache entfernt; ein normaler Bildklick erzeugt keinen bleibenden Auswahlzustand mehr.

### ✓ Technische Prüfung
- JavaScript-Syntax geprüft.
- Keine IndexedDB-Migration erforderlich.
- Bestehende Ideen-, Inspirations-, Ordner-, Buch- und Manuskriptdaten bleiben unverändert kompatibel.

## v1.2.2 – Inspirationsnavigation & Manuskript-Polish

### 🖼️ Inspirations-Kategorien
- Die dominanten Ordnersymbole wurden aus der Kategorienavigation entfernt.
- Haupt- und Unterkategorien werden jetzt ruhiger über Einrückung, Gewichtung und dezente Führungslinien unterschieden.
- Zähler sind rechts sauberer ausgerichtet; Hinzufügen- und Bearbeiten-Aktionen treten erst bei Hover/Fokus deutlicher hervor.
- Die aktive Kategorie verwendet einen neutralen warmen Auswahlstil statt eines auffälligen Farbrahmens.

### ✒️ Manuskript
- Der Schreibarbeitsplatz nutzt auf großen Viewports noch etwas mehr der verfügbaren Höhe und reduziert den bisher verbliebenen Leerraum unterhalb des Editors.
- Der Kopf des aktuellen Abschnitts um Titel, Fassung, Status und Schnellaktionen wurde deutlich flacher gesetzt.
- Versionszeile, Statusaktionen, Toolbar und Statusleiste wurden vertikal verdichtet, ohne Funktionen zu entfernen.
- Der Manuskripttext selbst bleibt in seiner bisherigen Breite; der gewonnene Platz kommt der sichtbaren Textmenge in der Höhe zugute.
- Die UI-Typografie im Schreibbereich wurde konsistenter hierarchisiert: Bedienelemente verwenden eine gemeinsame UI-Schrift, während Dokumenttitel und Manuskript bewusst in Serifenschrift bleiben.
- Der Fokusmodus nutzt die Viewport-Höhe ebenfalls noch konsequenter.

### ✓ Technische Prüfung
- Keine Datenbankmigration erforderlich.
- Keine Manuskript-, Inspirations- oder Storydaten verändert.

## v1.2.1 – Korrekturlauf

### 💡 Ideenarchiv

- Die Statistik-Karte `Dein Ideenraum` in der Seitenleiste wird beim Öffnen des neuen Ideen-Dashboards jetzt ebenfalls aus dem tatsächlich geladenen Ideenbestand aktualisiert.
- Bereits vorhandene Ideenzettel erscheinen dadurch wieder korrekt in Gesamt-, Wochen-, Monats- und Jahreszahlen; es werden keine Ideen migriert oder dupliziert.

### 📖 Buch-Dashboard

- Der künstliche helle/weiße Saum am Cover im Buch-Dashboard wurde entfernt.
- Das Cover behält weiterhin seinen dezenten Buchschatten, ohne zusätzliche helle Kontur.

### 🖼️ Inspiration

- Linksklick auf Inspirationsbilder öffnet die vorhandene Lightbox jetzt über einen eindeutigen Bild-Handler und wird nicht mehr von anderen Board-Interaktionen verschluckt.
- Der alte grünliche Fokus-/Auswahlrahmen beim normalen Anklicken wurde neutralisiert.
- Unterkategorien können beim Drag & Drop über einen deutlich sichtbaren Dropbereich `Auf Hauptebene verschieben` wieder aus ihrer Elternkategorie herausgezogen werden.
- Bestehendes Verschieben zwischen Kategorien bleibt erhalten.

### 👤 Story-Bibel · Bildergalerie

- Die permanenten Aktionen `Als Hauptbild` und `Entfernen` wurden aus der Galeriezeile entfernt.
- Rechtsklick auf das große Bild oder ein Thumbnail öffnet jetzt ein passendes Kontextmenü für `Als Hauptbild festlegen` und `Bild löschen`.
- Thumbnails können per Drag & Drop neu sortiert werden; die Reihenfolge wird mit dem Story-Bibel-Eintrag gespeichert.
- Interne Drag-&-Drop-Vorgänge werden von der Datei-Dropzone getrennt, damit ein bereits vorhandenes Galeriebild beim Verschieben nicht mehr dupliziert wird.
- Browser-eigenes Ziehen der Vorschaubilder wurde unterbunden.

### 👤 Charakterakte

- Das Vorschaubild im Kopf der Charakterakte wird jetzt vollständig innerhalb seines Bildbereichs dargestellt statt in einen festen Ausschnitt gezwungen zu werden.
- Der Bildbereich bleibt kompakt und erhält bei abweichendem Seitenverhältnis lediglich ruhigen Hintergrundraum.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine Datenbankmigration erforderlich.
- Keine Ideen-, Buch-, Inspirations-, Story-Bibel- oder Bilddaten strukturell verändert.

## v1.2.0 – Writing Experience & Workspace

### 🎨 Dashboard Studio
- Das allgemeine Dashboard besitzt jetzt eine eigene Anpassung, mit der zentrale Dashboard-Bausteine ein- und ausgeblendet werden können.
- Die Auswahl bleibt lokal gespeichert und verändert keine Projektdaten.

### 📖 Lesemodus
- Bücher können direkt vom Buch-Dashboard in einem ruhigen, editorfreien Lesemodus geöffnet werden.
- Der Lesemodus führt die vorhandenen Manuskriptabschnitte in Reihenfolge zusammen und zeigt Wortumfang und Abschnittszahl.

### 📈 Projektgeschichte & „Damals“
- Eine visuelle Projektchronik führt datierte Entstehungsmomente wie Buchanlage, Manuskriptabschnitte, Schreibsessions und Story-Growth-Ereignisse zusammen.
- „Damals an diesem Projekt“ greift ältere Momente separat auf, ohne daraus eine Produktivitätswertung zu machen.

### 🧪 Projekt-Gesundheitscheck
- Ein neuer organisatorischer Check weist unter anderem auf leere Szenen, fehlende POV-Zuordnungen und offene Story-Fäden hin.
- Der Check bewertet weder Stil noch Dramaturgie und repariert nichts automatisch.

### 📚 Buchvergleich innerhalb einer Reihe
- Bände derselben Reihe lassen sich kompakt nach Status, Wortumfang, Szenenzahl und POV-Anzahl vergleichen.
- Jeder Band kann aus dem Vergleich direkt geöffnet werden.

### 📦 Projekte archivieren
- Bücher können aus dem aktiven Arbeitsbestand archiviert und später wieder reaktiviert werden, ohne Inhalte zu löschen.
- Archivierte Bücher bleiben vollständig in der lokalen Datenbasis und im Suite-Backup erhalten.

### ✓ Technische Prüfung
- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine neue IndexedDB-Migration erforderlich.
- Bestehende Buch-, Manuskript-, Versions- und Storydaten bleiben kompatibel.

## v1.1.0

### ⌘ Command Palette & Global Search 2.0

- Die bereits vorhandene globale Suche wurde zum Connected-Workspace-Schnellstarter ausgebaut statt durch ein zweites Suchsystem ersetzt.
- Treffer können nach `Story & Schreiben`, `Wissen` und `Material` gefiltert werden.
- Romanideen sind jetzt ebenfalls direkt Bestandteil des globalen Suchindex.
- Treffer zeigen bei passenden Textfunden einen kompakten Kontextausschnitt und weiterhin die Zahl ihrer direkten Verknüpfungen.
- Häufig benötigte Inhalte können mit `☆` als persönliche Schnellzugriffe angepinnt werden und erscheinen beim Öffnen der Command Palette sofort oben.
- Persönliche Schnellzugriffe werden lokal gespeichert und in vollständigen Suite-Backups mitgeführt.

### 📥 Universal Inbox

- Die Command Palette besitzt jetzt eine Universal Inbox für Gedanken und beliebige Dateien, die noch keinem festen Bereich zugeordnet werden sollen.
- Inbox-Einträge können später bewusst zu Ideenzettel, Romanidee, Recherche oder – bei Bild/Video-Material – Inspiration weiterentwickelt werden.
- Bis zur Zuordnung bleiben Text und Dateien gemeinsam in der Inbox erhalten.
- Die Inbox verwendet den bereits vorhandenen `media`-Datenbereich und ist dadurch Teil vollständiger Backups; es wurde kein neues Datenbankschema benötigt.

### 📎 Zentrale Dateien-&-Medien-Ansicht

- Eine neue suiteweite Dateiansicht führt Anhänge aus Romanideen, Recherche, Inspiration, Story-Bibel, Buchmedien und Universal Inbox zusammen.
- Suche und Quellenfilter helfen auch bei wachsenden Projekten, Dateien wiederzufinden.
- `Herkunft` springt zurück zum eigentlichen Storyobjekt; die zentrale Ansicht erzeugt bewusst keine parallelen Dateikopien.
- Gespeicherte lokale Dateien können aus der Übersicht direkt geöffnet werden.

### ⌘ „Wo verwendet?“

- Die vorhandene Verknüpfungsanalyse bleibt die gemeinsame Grundlage und ist jetzt bei Story-Bibel-Einträgen direkt über `Wo verwendet?` erreichbar.
- Damit lässt sich unmittelbar nachvollziehen, in welchen Szenen, Ideen, Fäden, Timeline-Ereignissen, Rechercheeinträgen oder anderen Suite-Bereichen ein Wissenseintrag verwendet wird.
- Auch globale Suchtreffer führen weiterhin direkt in dieselbe Verknüpfungsansicht.

### ✂ Szenen teilen & zusammenführen

- Die bereits vorhandenen sicheren Aktionen zum Teilen und Zusammenführen von Manuskriptabschnitten bleiben erhalten und sind jetzt zusätzlich über die Command Palette auffindbar.
- Beim Teilen einer Szene erhält die neu entstehende Szene ausdrücklich eine eigene `sceneCoreId`; sie teilt damit nicht versehentlich denselben Szenenkern mit der Ursprungsszene.
- Planungskern-Zuordnungen werden beim Split nicht blind auf die neue Szene dupliziert.
- Sicherheits-Snapshots vor Teilen und Zusammenführen bleiben unverändert aktiv.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- HTML auf doppelte IDs geprüft.
- Keine IndexedDB-Migration erforderlich.
- Bestehende Command-Palette-, Global-Links-, Szenen-, Recherche-, Inspirations- und Mediendaten bleiben kompatibel.

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
