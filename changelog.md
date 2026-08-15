# 📝 Ideenarchiv -- Changelog

Alle wichtigen Änderungen und Entwicklungsschritte des **Ideenarchivs**.

> Das Ideenarchiv wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt.
> Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

---
## v0.80.45

### 🏠 Startseite – Ursache an der Wurzel behoben

Die leere „Alle Ideen“-Ansicht konnte weiterhin als Startseite sichtbar
bleiben, obwohl die Routing-Logik anschließend eigentlich das Dashboard
öffnen sollte.

Die Ursache lag tiefer: Das Ideenarchiv war im rohen HTML weiterhin die
standardmäßig sichtbare Ansicht. Außerdem wurde vor der eigentlichen
Startnavigation noch `renderAll()` sowie mehrere Audits ausgeführt.

Wenn in dieser Phase etwas abbrach oder verzögert wurde, blieb genau diese
rohe Archivansicht stehen.

### Neue robuste Startarchitektur

Die Startreihenfolge wurde geändert:

1. Daten laden
2. Kernbindings aufbauen
3. Startansicht sofort bestimmen und rendern
4. erst danach optionale Audits, Sicherheitschecks und Integritätsprüfungen

Dadurch kann ein Fehler in einem Nebenmodul die Startnavigation nicht mehr
verhindern.

### HTML-Fallback ebenfalls korrigiert

Schon bevor JavaScript fertig geladen ist, ist jetzt:

- Dashboard die Standardansicht
- Ideenarchiv ausgeblendet
- Archiv-Topbar ausgeblendet
- Archiv-Toolbar ausgeblendet
- Archiv-Bottom-Controls ausgeblendet

Selbst wenn JavaScript beim Start unerwartet abbrechen sollte, fällt die
Seite damit nicht mehr auf „Alle Ideen“ zurück.

### ↻ F5

Das bestehende F5-Verhalten bleibt erhalten:

- frischer Start → Dashboard
- echter Reload derselben Tab-Sitzung → letzter Arbeitskontext

### 💾 Daten

Keine gespeicherten Inhalte wurden verändert.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Dashboard im Roh-HTML sichtbar
- Ideenarchiv im Roh-HTML verborgen
- Startnavigation läuft vor Audits
- `renderAll()` aus dem globalen Startup entfernt
- Audits können die Startnavigation nicht mehr abbrechen
- `showDashboard()` schließt Archiv-Chrome defensiv
- keine doppelten statischen HTML-IDs

## v0.80.44

### 🎬 Videos im Inspiration Board

Inspiration unterstützt jetzt neben Bildern auch lokale Videodateien.

Videos können genau wie Bilder:

- über `＋ Medien` ausgewählt werden
- direkt auf das Inspirationsboard gezogen werden
- direkt auf einen Inspirationsordner gezogen werden
- zwischen Ordnern verschoben werden
- mit einer optionalen Caption versehen werden
- gelöscht werden

### ▶ Direkt im Masonry abspielen

Videos fügen sich als normale Kacheln in das bestehende Masonry-Raster ein.

Sie besitzen direkt auf dem Board den nativen Browser-Player mit:

- Play / Pause
- Zeitleiste
- Lautstärke
- Vollbild, sofern der Browser es unterstützt

Das Video muss zum Abspielen nicht erst in einem separaten Fenster
geöffnet werden.

Die Kachelhöhe wird nach dem Laden der Video-Metadaten neu berechnet,
damit Videos mit unterschiedlichen Seitenverhältnissen sauber im
Masonry bleiben.

### ↕ Verschieben ohne Player-Konflikt

Damit die Videosteuerung benutzbar bleibt, wird ein Medium nicht mehr
durch Ziehen auf dem eigentlichen Bild/Video verschoben.

Jede Kachel besitzt jetzt einen kleinen `⠿`-Ziehgriff.

So lassen sich:

- Videos normal bedienen
- Bilder normal anklicken
- beide Medientypen trotzdem zuverlässig per Drag & Drop verschieben

### 💾 Lokale Videos & Speichergröße

Videos werden lokal im Browser gespeichert, genau wie die übrigen
Suite-Daten.

Da Videodateien deutlich größer als Bilder sein können:

- ab 40 MB wird vor dem Speichern nachgefragt
- über 120 MB wird eine Datei nicht in das Inspiration Board übernommen

### 🔄 Kompatibilität

Bestehende Inspirationsbilder bleiben unverändert kompatibel.

Neue Medieneinträge ergänzen das Datenmodell um:

- `mediaKind`
- `mediaData`
- `mediaMime`
- `mediaSize`

Alte Bilddaten verwenden weiterhin `imageData`.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Bild- und Videodateien im gemeinsamen Board
- Video-Drop auf Board und Ordner
- Inline-Player mit Browser-Controls
- Masonry-Neuberechnung nach `loadedmetadata`
- Caption und Ordnerwechsel für Videos
- Medien löschen
- separater Drag-Griff verhindert Konflikte mit Video-Controls
- bestehende Bilder bleiben kompatibel
- keine doppelten statischen HTML-IDs

## v0.80.43

### 🏠 Startseite endgültig vom Ideenarchiv entkoppelt

Die Autoren-Suite verwendet für die Start-/Reload-Erkennung keinen
`#section`-Hash mehr.

Genau dieser Hash konnte bei lokal geöffneten `index.html`-Dateien hängen
bleiben und beim nächsten Start wieder das Ideenarchiv öffnen, obwohl das
Dashboard die eigentliche Startseite ist.

### Neue Startlogik

Jetzt gilt:

- `index.html` normal/frisch öffnen → immer globales Dashboard
- alter `#section=ideas`-Hash → wird beim normalen Start entfernt
- innerhalb der Suite navigieren → URL wird nicht mehr verändert
- echtes `F5` im selben Browser-Tab → letzter Arbeitskontext wird restauriert
- Logo → Dashboard

Für F5 wird jetzt nur noch eine Kombination aus:

- echtem Browser-Navigationstyp `reload`
- derselben Tab-Sitzung

verwendet.

Damit ist ein gespeicherter Bereich allein nicht mehr in der Lage,
sich zur Startseite zu machen.

### 💾 Daten

Ideen, Kategorien, Bücher und andere Daten werden nicht verändert.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Startlogik verwendet keinen Bereichs-Hash mehr
- Dashboard räumt alte Hashes auf
- F5-Resume bleibt über Tab-Sitzung erhalten
- frischer Start fällt immer auf Dashboard zurück
- keine doppelten statischen HTML-IDs

## v0.80.42

### ♧ Inspiration – Raster, Löschen & Drag-and-Drop korrigiert

### ▦ Zeilenweise Masonry-/Kachelansicht

Das Inspirationsboard füllt jetzt zuerst von links nach rechts auf.

Erst wenn eine Zeile belegt ist, beginnt die nächste. Unterschiedlich hohe
Bilder bleiben trotzdem unterschiedlich hoch; kleinere Karten können freie
Lücken darunter dicht auffüllen.

Technisch wurde die bisherige CSS-Spaltenansicht durch ein
zeilenorientiertes, dichtes Grid ersetzt.

### 🗑 Bilder wieder zuverlässig löschen

Inspirationsbilder können jetzt:

- direkt über `×` auf der Bildkarte gelöscht werden
- weiterhin im Bearbeitungsdialog gelöscht werden

Die Löschlogik verwendet jetzt eine gemeinsame Funktion und vergleicht IDs
robust als Strings.

### ↕ Keine Kopien mehr beim internen Drag & Drop

Beim Verschieben eines bereits gespeicherten Inspirationsbildes wurde der
interne Drag teilweise zusätzlich als Datei-Drop interpretiert.

Dadurch entstand beim Loslassen im selben Ordner eine ungewollte Kopie.

Das ist behoben:

- interne Bild-Drags werden eindeutig erkannt
- interne Drags werden niemals als neuer Datei-Upload verarbeitet
- Drop in denselben Ordner ist ein No-op
- nur echte Dateien von außerhalb der Suite erzeugen neue Bilder
- der native Browser-Drag des eigentlichen Bildes wurde deaktiviert

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- zeilenorientiertes Dense-Grid aktiv
- dynamische Grid-Höhenberechnung vorhanden
- direktes Löschen und Dialog-Löschen nutzen dieselbe Funktion
- interne Drag-Typen werden vor Datei-Drops ausgewertet
- Same-Folder-Drop erzeugt keine neue Kopie
- keine doppelten statischen HTML-IDs

## v0.80.41

### 🏠 Startlogik erneut korrigiert – Dashboard ist jetzt die echte Startseite

Die Start-/Reload-Erkennung wurde noch einmal grundsätzlich vereinfacht.

Statt browserabhängiger `performance.navigation`-/History-Erkennung verwendet
die Suite jetzt einen eigenen URL-Routen-Hash.

Damit gilt eindeutig:

- `index.html` frisch öffnen → Dashboard
- innerhalb der Suite navigieren → Route wird im Hash gespeichert
- `F5` → aktuelle Unterseite bzw. gespeicherter Arbeitskontext wird wiederhergestellt
- Klick auf das Logo → Dashboard und Routen-Hash wird entfernt

Ein frisch geöffnetes lokales `file://`-Dokument kann dadurch nicht mehr
versehentlich als Reload des Ideenarchivs interpretiert werden.

### 💡 Einfache Ideenanlage – Dublettenprüfung sichtbar gemacht

Die bestehende Prüfung auf bereits vorhandene Ideen wurde ausgebaut und ist
jetzt bei der normalen einzelnen Ideenanlage eindeutig sichtbar.

Während des Tippens wird angezeigt:

- `⚠ exakt vorhanden` bei identischer Idee
- `≈ möglicherweise ähnlich` bei ausreichend hoher Textähnlichkeit
- `✓ Neue Idee` wenn keine exakte oder ausreichend ähnliche Idee gefunden wird

Bei exakten und ähnlichen Treffern werden Fundort und vorhandene Idee
angezeigt und können direkt geöffnet werden.

Sehr kurze Eingaben zeigen zunächst sichtbar, dass die Dublettenprüfung aktiv
ist, ohne vorschnell eine Ähnlichkeitsbewertung zu behaupten.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- frischer Start ohne Hash → Dashboard
- F5 mit Suite-Hash → Resume-Kontext
- Dashboard entfernt den Hash
- Ideenprüfung nutzt exakte und Similarity-Prüfung
- sichtbarer Status auch bei neuer Idee
- keine doppelten statischen HTML-IDs

## v0.80.40

### 🖼 Bildverarbeitung repariert

Beim direkten Hineinziehen von Bildern in Inspiration erschien die Meldung
„Bild konnte nicht verarbeitet werden“.

Ursache war eine fehlende zentrale Bildfunktion (`resizeImageFile`), die von
Inspiration, Recherche, Charakterbildern, Buchcovern und weiteren
Medienbereichen bereits aufgerufen wurde.

### ✓ Reparatur

Die gemeinsame Bildverarbeitung ist wieder vorhanden und übernimmt jetzt:

- PNG
- JPEG/JPG
- WEBP
- GIF als statisches Bild
- SVG ohne Rasterung
- proportionale Verkleinerung großer Bilder
- Erhalt transparenter Hintergründe bei geeigneten Formaten
- Fallback auf die Originaldatei, falls nur die Optimierung fehlschlägt

### ♧ Inspiration

Bilder können wieder direkt:

- auf das Pinterest-artige Inspirationsboard gezogen werden
- auf einen Inspirationsordner gezogen werden
- über `＋ Bilder` ausgewählt werden
- mit `Strg+V` eingefügt werden

Nach erfolgreicher Verarbeitung werden sie sofort gespeichert und angezeigt.

### 🔧 Nebenwirkung positiv

Da dieselbe fehlende Funktion auch andere Bildbereiche verwendeten, sind
damit zugleich die betroffenen Upload-Pfade für Recherche, Charakterbilder,
Ortsbilder und Buchcover wieder funktionsfähig.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- `resizeImageFile` wieder zentral definiert
- Inspiration verwendet die gemeinsame Bildpipeline
- Fallback auf Original-Data-URL vorhanden
- keine doppelten statischen HTML-IDs

## v0.80.39

### 🏠 Startseite wieder eindeutig das Dashboard

Beim frischen Öffnen der `index.html` startet die Autoren-Suite jetzt
wieder zuverlässig auf dem globalen Dashboard.

Der zuletzt geöffnete Bereich wird beim normalen Start nicht mehr
versehentlich als Startseite übernommen.

### ↻ F5 bleibt trotzdem erhalten

Das gewünschte Reload-Verhalten bleibt bestehen:

- `index.html` frisch öffnen → Dashboard
- innerhalb der Suite zu Recherche, Inspiration, Ideenarchiv,
  Buch usw. wechseln
- `F5` / Browser-Neuladen → auf genau diesem Arbeitsbereich bleiben

Dafür wird jetzt zusätzlich die History des aktuell geöffneten
Dokuments geprüft.

Der bisherige alleinige Check über `performance.navigation` war bei
lokalen HTML-Dateien nicht zuverlässig und konnte ein frisch
geöffnetes `file://`-Dokument fälschlich als Reload behandeln.

### ✒ Logo / Startseite

Ein Klick auf das Logo führt weiterhin ausdrücklich zum globalen
Dashboard.

### 💾 Daten

An Ideen, Büchern, Manuskripten oder anderen gespeicherten Daten wird
nichts verändert.

Die Korrektur betrifft ausschließlich die Start-/Reload-Navigation.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- frischer Start → Dashboard
- Reload-Kontext bleibt separat erhalten
- Logo → Dashboard
- keine doppelten statischen HTML-IDs

## v0.80.38

### ♧ Inspiration neu gedacht – echtes visuelles Board

Der Inspirationsbereich wurde grundlegend vereinfacht.

Er ist jetzt bewusst kein Formular-Modul mehr, sondern ein visuelles
Bildarchiv ähnlich einem Pinterest-Board.

### 🖼 Bilder direkt sammeln

Bilder können jetzt direkt:

- per Drag & Drop auf das Board gezogen werden
- per Drag & Drop direkt auf einen Ordner gezogen werden
- über `＋ Bilder` ausgewählt werden
- mit `Strg+V` aus der Zwischenablage eingefügt werden

Jedes Bild wird sofort als eigener Inspirationseintrag gespeichert.

Es ist kein zusätzlicher Speichern-Dialog mehr nötig.

### 📌 Pinterest-artige Ansicht

Die Inspirationen werden in einer bildorientierten Masonry-/Pinterest-
Ansicht dargestellt.

- unterschiedliche Bildformate behalten ihre natürliche Höhe
- keine einheitlichen künstlichen Kartenhöhen
- Bilder werden nicht gestreckt
- mehrere Spalten nutzen den verfügbaren Platz
- lange und kurze Bilder können frei nebeneinander stehen

### ✎ Caption statt Formular

Ein Bild benötigt keine Metadaten.

Optional kann per Klick auf das Bild eine kurze Caption hinterlegt werden.

Diese Caption erscheint direkt unter dem Bild.

Im kleinen Bearbeitungsdialog können nur noch:

- Caption
- Ordner

geändert werden.

Außerdem kann das Bild dort gelöscht werden.

### 📁 Ordner bleiben erhalten

Die verschachtelten Inspirationsordner bleiben vollständig erhalten.

Zusätzlich können jetzt:

- neue Bilder direkt in einen geöffneten Ordner geladen werden
- Bilder per Drag & Drop zwischen Ordnern verschoben werden
- Bilder auf „Alle Inspirationen“ gezogen und damit aus einem Ordner
  gelöst werden
- Ordner weiterhin verschachtelt werden

### 📋 Zwischenablage-Bug behoben

Der bisherige Clipboard-Handler zeigte zwar „Bild eingefügt“, legte bei
Inspiration aber nur einen noch nicht gespeicherten Formularzustand an.

Inspiration speichert Zwischenablagebilder jetzt sofort direkt ins Board.

Damit ist nach `Strg+V` das Bild unmittelbar sichtbar und dauerhaft
gespeichert.

Charakter- und Recherche-Dialoge behalten ihre bisherige Clipboard-Logik.

### 🧹 Inspiration und Recherche wieder klar getrennt

Die komplexeren Funktionen wie Quellen, Dateien, eigene Felder und
strukturierte Recherche bleiben im Recherchebereich.

Inspiration ist dagegen bewusst visuell und leichtgewichtig.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- direkte Bildspeicherung bei Drag & Drop
- direkte Bildspeicherung bei Strg+V
- Datei-Mehrfachauswahl unterstützt
- Masonry-/Pinterest-Board aktiv
- Caption optional
- Bildverschiebung zwischen Ordnern per Drag & Drop
- Ordner-Verschachtelung bleibt erhalten
- keine doppelten statischen HTML-IDs

## v0.80.37

### 📋 Bilder aus der Zwischenablage

Bildbereiche unterstützen jetzt `Strg+V`.

Unterstützt werden:

- Charakter-Hauptgalerie
- zusätzliche benennbare Charaktergalerien
- Inspiration-Hauptbild
- Inspiration-Bildergalerie
- Recherche-Hauptbild
- Recherche-Bildergalerie

### 🎯 Aktive Bildfläche

Die zuletzt angeklickte oder fokussierte Bildfläche ist das Einfügeziel.

Beispiel:

1. Charakter öffnen
2. Galerie „Outfits“ anklicken
3. Bild kopieren
4. `Strg+V`
5. Bild landet in „Outfits“

Die aktive Fläche wird dezent hervorgehoben.

### ↩ Standardziel

Wenn noch keine Bildfläche gewählt wurde:

- Charakter → Hauptgalerie
- Inspiration vom Typ Bild → Hauptbild
- andere Inspirationstypen → Galerie
- Recherche vom Typ Bild → Hauptbild
- andere Recherchetypen → Galerie

### 🖼 Gemeinsame Bildverarbeitung

Zwischenablagebilder laufen durch dieselbe Verarbeitung wie Upload
und Drag & Drop.

Größenoptimierung, Galerielogik und proportionale Darstellung
bleiben erhalten.

Normales Text-Einfügen wird nicht abgefangen.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- zentraler Clipboard-Image-Handler
- aktive Zielgalerie unterstützt
- Standardziel je Dialog vorhanden
- Charakter, Inspiration und Recherche unterstützt
- Text-Paste bleibt unangetastet
- keine doppelten statischen HTML-IDs

## v0.80.36

### ◉ Recherche Studio – Sammlungen, Bücher & Medien

Der Recherchebereich wurde analog zum Inspiration Studio ausgebaut.

### 📁 Verschachtelte Recherchesammlungen

- Sammlungen und Untersammlungen
- Drag & Drop für Recherchen und Sammlungen
- „Alle Recherchen“ als Hauptebene
- Suche bleibt sammlungsübergreifend
- Löschen einer Sammlung löscht keine Rechercheeinträge

### 📚 Mehrfach-Buchzuordnung

Jede Recherchesammlung kann optional mit einem oder mehreren
konkreten Büchern verknüpft werden.

Ohne Buchauswahl bleibt sie projektweit verfügbar.

Die Rechercheauswahl im Manuskript berücksichtigt diese Zuordnung.

### 🖼 Medien & Dateien

- Hauptbild per Drag & Drop
- optionale Bildergalerie
- Slider
- Vorschaubilder
- Bildnotizen
- PDFs, Scans und andere Dateianhänge
- eigene frei benennbare Felder

### 💾 Datenmodell

Rechercheeinträge:

- collectionId
- gallery
- files
- customFields

Sammlungen:

- parentCollectionId
- projectId
- bookIds

Bestehende Rechercheeinträge bleiben kompatibel.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Sammlungen nicht als Rechercheeinträge gezählt
- Verschachtelung und Drag & Drop unterstützt
- Mehrfach-Buchzuordnung unterstützt
- Manuskript-Rechercheauswahl berücksichtigt Buchzuordnung
- Galerie, Dateien und eigene Felder unterstützt
- keine doppelten statischen HTML-IDs

## v0.80.35

### ♧ Inspirationssammlungen – Plus-Button repariert

- Das `+` bei Sammlungen öffnet den Dialog zum Anlegen eines Ordners
  jetzt zuverlässig.
- Die Aktion ist zusätzlich über die zentrale, robuste Event-Delegation
  abgesichert.
- Das Speichern des Sammlungs-/Ordnerdialogs ist ebenfalls zentral
  abgesichert.
- Unterordner anlegen, Ordner öffnen und Ordner bearbeiten laufen über
  dieselbe robuste Ereignisbehandlung.
- „Alle Inspirationen“ und „Inspiration hier anlegen“ wurden ebenfalls
  abgesichert.
- JavaScript-Syntax geprüft.

## v0.80.34

### 👤 Charaktere – optionale Ordner & Gruppen

Charaktere können jetzt optional in Ordnern organisiert werden.

Ohne angelegte Ordner bleibt die bisherige flache Charakterliste erhalten.

### 📁 Ordner

Im Charakterbereich können jetzt:

- Ordner angelegt werden
- Unterordner angelegt werden
- Ordner umbenannt werden
- Ordner gelöscht werden
- beliebig verschachtelte Gruppen aufgebaut werden

Die Ordner verwenden die bestehende Wissen-&-Welt-Ordnerstruktur
der Charaktereinträge.

### ↕ Drag & Drop

Charakterkarten sind jetzt verschiebbar.

Per Drag & Drop können Charaktere:

- in einen Ordner gezogen werden
- in einen Unterordner gezogen werden
- zurück auf „Alle Charaktere“ gezogen werden

Auch Ordner selbst können per Drag & Drop unter andere Ordner
verschoben oder wieder auf die Hauptebene gezogen werden.

Zirkuläre Verschachtelungen werden verhindert.

### 🗑 Löschen eines Ordners

Beim Löschen eines Ordners werden die enthaltenen Charaktere nicht gelöscht.

Sie werden eine Ebene nach oben verschoben. Unterordner bleiben ebenfalls
erhalten und werden entsprechend nach oben gehängt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- bestehende folderId-Struktur wiederverwendet
- optionale flache Darstellung bleibt erhalten
- Charakter-Drag-&-Drop gebunden
- Ordner-Drag-&-Drop gebunden
- Unterordner unterstützt
- keine doppelten statischen HTML-IDs

## v0.80.33

### ♧ Inspiration Studio ausgebaut

Der Inspirationsbereich kann jetzt sowohl visuell als auch strukturiert
genutzt werden.

### 📁 Verschachtelte Ordner

- Ordner und Unterordner anlegen
- beliebig verschachtelte Hierarchie
- Ordner umbenennen und verschieben
- Projektzuordnung für Ordner
- Inspiration direkt in einen Ordner anlegen
- Suche bleibt ordnerübergreifend
- beim Löschen eines Ordners bleiben die Inspirationen erhalten

### 🖼 Bilder & Drag and Drop

- Hauptbild per Dateiauswahl oder Drag & Drop
- optionale Bildergalerie
- mehrere Bilder
- Slider
- Vorschaubilder
- Bildnotizen
- proportionale Darstellung ohne Strecken

### 📎 Dateien

PDFs, Dokumente und andere Dateien können per Auswahl oder Drag & Drop
angehängt, angezeigt, entfernt und lokal gespeichert werden.

### ✎ Eigene Felder

Jede Inspiration kann beliebig viele frei benennbare Felder besitzen.

### 💾 Datenmodell

Bestehende Inspirationen bleiben kompatibel. Ergänzt wurden:

- folderId
- gallery
- files
- customFields

Ordner werden im bestehenden Inspirationsspeicher verwaltet und sind
dadurch automatisch Teil von Backup und Export.

### 🧹 Kleine Bereinigung

Ein versehentlich im Inspirationsdialog vorhandenes „Reiter“-Feld aus
der Anhang-/Story-Bibel-Logik wurde entfernt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Ordner und Unterordner unterstützt
- Ordner werden nicht als Inspirationen gezählt
- Hauptbild unterstützt Drag & Drop
- Galerie unterstützt Drag & Drop
- Dateien unterstützt
- eigene Felder unterstützt
- keine doppelten statischen HTML-IDs

## v0.80.32

### 🐞 Buch-Dashboard – Ladefehler behoben

Beim Öffnen eines Buches konnte die neue Buchübersicht mit
„Dieser Bereich ist gerade gestolpert“ abbrechen.

Ursache war ein veralteter bzw. nicht vorhandener Helper-Aufruf
für die Ortsdaten im neuen Dashboard.

### 🔗 Wissen & Welt

Die Ortsanzahl der Buchübersicht wird jetzt direkt aus den
für das jeweilige Buch sichtbaren Wissen-&-Welt-Einträgen ermittelt.

Dabei werden:

- nur Einträge vom Typ „Ort“ berücksichtigt
- die Buchfreigaben beachtet
- mehrfach erreichbare Einträge dedupliziert

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- nicht definierter `knowledgeEntriesForBook`-Aufruf entfernt
- Dashboard-Abhängigkeiten auf vorhandene Funktionen geprüft
- keine doppelten statischen HTML-IDs

## v0.80.31

### 🏠 Buch-Dashboard & Navigation – Konsistenzrunde

Die Buchübersicht wurde grundlegend überarbeitet.
Sie wiederholt nicht mehr die komplette obere Navigation.

Das Dashboard konzentriert sich jetzt auf:

- aktuellen Buchstatus
- Schreib- und Planungsstand
- Wissen-&-Welt-Status
- zuletzt bearbeitete Manuskriptabschnitte
- offene Handlungsfäden
- Story-Intelligence-Hinweise
- Kontinuität
- Wochenfortschritt
- persönliche Notiz zum nächsten Arbeitsschritt

Die eigentlichen Arbeitsbereiche bleiben bewusst
in der oberen Navigation.

### 👤 Charaktere

- „Steckbrief & Medien“ heißt jetzt nur noch „Steckbrief“.
- Ein Fehler in der Steckbriefansicht wurde behoben,
  durch den der Charakterbereich mit
  „Dieser Bereich ist gerade gestolpert“ abbrechen konnte.
- Die Beziehungsanzeige im Charaktersteckbrief verwendet
  jetzt das aktuelle Beziehungs-Datenmodell.

### 🧭 Informationshierarchie

Das Buch-Dashboard ist jetzt Status- und
Fortsetzungszentrale statt zweiter Menüleiste.

Direkte Links im Dashboard werden nur noch dort verwendet,
wo sie aus einem konkreten Status oder offenen Punkt entstehen.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Charakter-Steckbrief-Renderer auf aktuelles
  Beziehungsmodell korrigiert
- Dashboard-Navigation entdoppelt
- vorhandene Arbeitsbereiche bleiben über die
  obere Navigation erreichbar
- Dashboard-Aktionslinks geprüft
- keine doppelten statischen HTML-IDs

## v0.80.30

### 👤 Charaktere – Steckbrief und Entwicklung getrennt

Der Charakterbereich besitzt jetzt zwei klare Ansichten:

- Steckbrief & Medien
- Entwicklung

Beim Öffnen eines Charakters ist „Steckbrief & Medien“
standardmäßig aktiv.

### 📋 Steckbrief & Medien

Die Standardansicht zeigt direkt:

- Name
- Untertitel
- Aliase
- Kurzbeschreibung
- Hauptbild
- Haupt-Bildergalerie
- sämtliche ausgefüllten Steckbrief-Felder
- freie Notizen
- zusätzliche benannte Bildergalerien
- Dateianhänge
- vorhandene Beziehungen

Die Charakterakte kann direkt über:

> ✎ Charakter bearbeiten

geöffnet werden.

### 🖼 Galerien in der Leseansicht

Die Hauptgalerie und zusätzliche Charaktergalerien besitzen:

- feste Bildfläche
- proportionale Darstellung
- Vor-/Zurück-Navigation
- Vorschaubilder
- Bildnotiz

### ↗ Entwicklung

Der zweite Reiter enthält die bisherige Character-Arc-Ansicht:

- Entwicklungs-Timeline
- Anfang vs. aktueller Stand
- Story-Kontext
- Manuskriptverknüpfungen
- Entwicklungsstationen
- Szenenbezug
- Timeline-Bezug

### 🧭 Charakterwechsel

Beim Wechsel auf einen anderen Charakter wird automatisch
wieder der Steckbrief geöffnet.

Auch nach dem Anlegen oder Bearbeiten eines Charakters landet
die Ansicht wieder im Steckbrief.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Steckbrief als Standardansicht
- Entwicklung als separater Reiter
- gespeicherte Steckbrief-Felder werden dargestellt
- Hauptgalerie wird dargestellt
- zusätzliche benannte Galerien werden dargestellt
- Dateien werden dargestellt
- Beziehungen werden zusammengefasst
- Charakterwechsel setzt auf Steckbrief zurück
- keine doppelten HTML-IDs

## v0.80.29

### 👤 „Figurenbogen“ heißt jetzt „Charaktere“

Der Bereich unter Wissen & Welt wurde in der sichtbaren Navigation
von „Figurenbogen“ zu „Charaktere“ umbenannt.

Auch der Schnellzugriff auf dem Buch-Dashboard verwendet jetzt
„Charaktere“.

Die interne Workflow-ID `characterarc` bleibt aus
Kompatibilitätsgründen unverändert.

### 🖼 Drag & Drop für Bilder

Bilder können jetzt direkt in die Hauptgalerie gezogen werden.

Die bestehende Galerie behält:

- feste Darstellungsfläche
- proportionale Darstellung
- Slider
- Vorschaubilder
- Hauptbild
- Bildnotizen

Bilder können weiterhin auch über `＋ Bilder` ausgewählt werden.

### 📎 Drag & Drop für andere Dateien

Wissensakten unterstützen jetzt allgemeine Dateianhänge.

Dateien können:

- per Drag & Drop hinzugefügt
- über `＋ Dateien` ausgewählt
- mit Dateiname, Typ und Größe angezeigt
- wieder entfernt
- lokal aus der Akte gespeichert werden

Bilder werden automatisch erkannt und der Bildergalerie zugeordnet.

Dateien über 10 MB erzeugen eine Warnung.
Dateien über 25 MB werden zum Schutz des lokalen Speichers
nicht aufgenommen.

### 🖼 Benennbare zusätzliche Bildergalerien für Charaktere

Charaktere können beliebig viele weitere Galerien besitzen.

Über:

`＋ Galerie`

kann z. B. angelegt werden:

- Outfits
- Kindheit
- Familie
- Referenzen
- Beziehungen
- unterschiedliche Lebensphasen

Jede Galerie besitzt:

- frei editierbaren Namen
- eigenen Slider
- feste Bildfläche
- proportionale Bilddarstellung
- Vorschaubilder
- Bildnotizen
- Bilder-Upload
- Drag & Drop
- einzelne Bilder löschen
- komplette Galerie löschen

### 💾 Datenmodell

Wissenseinträge speichern jetzt zusätzlich:

- `imageGalleries`
- `files`

Bestehende Einträge bleiben kompatibel.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- „Figurenbogen“ zu „Charaktere“ umbenannt
- Hauptgalerie unterstützt Drag & Drop
- zusätzliche Charaktergalerien unterstützt
- Galerien frei benennbar
- Dateien per Upload und Drag & Drop unterstützt
- bestehende Bilder bleiben kompatibel
- keine doppelten HTML-IDs

## v0.80.28

### 🖱 Mausbuttons für Zurück & Vor

Die typischen Daumentasten einer Maus werden jetzt innerhalb der
Autoren-Suite unterstützt:

- Mausbutton „Zurück“ → vorherige Ansicht
- Mausbutton „Vor“ → nächste Ansicht

Die Navigation arbeitet mit einem eigenen Verlauf der Suite und springt
deshalb nicht einfach aus der Anwendung heraus.

Berücksichtigt werden unter anderem:

- Hauptbereiche
- Ideenarchiv
- Bücher / Schreibansicht
- Buch-Workflow-Reiter
- Projektansichten
- aktuell geöffnetes Manuskript
- Mindmap-Kontext

Neue Navigation nach einem Zurück-Sprung verwirft – wie bei einem
normalen Browser – den bisherigen Vorwärts-Verlauf.

Der interne Verlauf ist auf 100 Navigationsstände begrenzt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Mausbutton 3 als „Zurück“ gebunden
- Mausbutton 4 als „Vor“ gebunden
- Browser-Standardaktion der Daumentasten innerhalb der Suite abgefangen
- eigener Vor-/Zurück-Verlauf vorhanden
- F5-/Resume-Logik bleibt separat erhalten
- keine doppelten HTML-IDs

## v0.80.27

### 👤 Charaktere – zeitliche Einordnung entfernt

Der Block „Zeitliche Einordnung · optional“ wird bei Figuren nicht mehr angezeigt.

Für eine Figurenakte war eine einzelne Zuordnung zu Datum, Uhrzeit und
Handlungsstrang fachlich irreführend, weil die Entwicklung einer Figur
bereits über Character Arc, Szenen und Timeline verfolgt wird.

Bei anderen Wissenseinträgen bleibt die zeitliche Einordnung weiterhin
verfügbar, wenn sie dort sinnvoll ist.

### 🖼 Bildergalerie direkt oben

Die Bildergalerie sitzt im Wissenseintragsdialog jetzt direkt unter Titel
und Überschrift.

Damit sind Portraits und visuelle Referenzen bei Figuren sofort sichtbar.

### ↔ Galerie als Slider

Mehrere Bilder werden als echte Galerie dargestellt:

- Pfeil links / rechts
- Bildzähler
- klickbare Vorschaubilder
- Bildnotiz
- Hauptbild festlegen
- Bild entfernen

Beim Öffnen einer vorhandenen Akte startet die Galerie nach Möglichkeit
mit dem Hauptbild.

### 📐 Unterschiedliche Bildgrößen ohne springendes Layout

Die Galerie besitzt eine feste Bildfläche.

Bilder werden:

- nicht gestreckt
- nicht verzerrt
- nicht automatisch beschnitten
- mittig ausgerichtet
- proportional so groß wie möglich dargestellt

Dadurch bleibt die Oberfläche gleich groß, egal ob ein Bild Hochformat,
Querformat oder quadratisch ist.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Zeitblock bei Charakteren ausgeblendet
- Galerie sitzt nur einmal im Dialog
- Galerie direkt oben
- Slider-Navigation gebunden
- Hauptbild-/Löschen-Funktionen erhalten
- Bildnotizen werden weiterhin gespeichert
- Mehrfachupload bleibt erhalten
- keine doppelten HTML-IDs

## v0.80.26

### 🛠 Figuren- und Ortsaktionen repariert

Die sichtbaren neuen Verwaltungsbuttons aus dem Wissen-&-Welt-Umbau
waren teilweise noch nicht mit den neuen Aktionen verbunden.

### 👤 Figuren

Repariert:

- + Figur
- + in der Figuren-Seitenleiste
- + Erste Figur anlegen
- ✎ Figur bearbeiten

Zusätzlich öffnet ein Doppelklick auf eine Figur in der Liste
direkt ihre Akte.

Die vorhandenen Links für Entwicklungsstationen, Szenen und
Timeline bleiben erhalten.

### 🗺 Orte / Weltkarte

Der Stift neben einem Ort ist jetzt tatsächlich mit
„Ortsakte bearbeiten“ verbunden.

Aktionen sind sauber getrennt:

- Ortszeile anklicken → Ort zum Platzieren auswählen
- Stift → Ortsakte bearbeiten
- Drag & Drop → Ort auf Karte ziehen
- + Ort → neuen Ort anlegen

Der Stift löst nicht mehr gleichzeitig die Kartenplatzierung aus.

### 🛡 Wissensdialoge robuster

Beim Öffnen von Figuren, Orten und anderen Wissenseinträgen werden
Fehler abgefangen. Wenn ein Datensatz nicht gefunden wird, zeigt
die Suite einen Hinweis statt eines scheinbar toten Links.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Figuren-Neuanlage gebunden
- Figuren-Bearbeiten gebunden
- Figuren-Doppelklick gebunden
- Orts-Stift gebunden
- Ortsauswahl und Bearbeiten getrennt
- keine doppelten HTML-IDs

## v0.80.24

### 👤 Figurenbogen wird zur Figurenverwaltung

Figuren werden nach der Trennung von Wissen & Welt und Anhang
jetzt direkt im Figurenbogen angelegt und verwaltet.

Neu:

- Figur anlegen
- Figur bearbeiten
- Figuren suchen und auswählen
- Steckbrief, Aliase, Bilder und weitere Felder pflegen
- Entwicklungsstationen anlegen und bearbeiten
- Szenen- und Timeline-Bezug verfolgen

Der leere Zustand führt nicht mehr zum Anhang,
sondern direkt zu „Erste Figur anlegen“.

### 🗺 Weltkarte verwaltet Orte direkt

- + Ort öffnet die Ortsakte direkt
- vorhandene Orte besitzen eine Bearbeiten-Aktion
- Kartenpins öffnen Ortsakten direkt
- Figuren-Overlays öffnen Figurenakten direkt
- kein unnötiger Wechsel in den Anhang

### 🧠 Codex verwaltet Wissenseinträge direkt

Der frühere Story-Bibel-Link wurde durch
„+ Wissenseintrag“ ersetzt.

- neue Wissenseinträge direkt im Codex
- vorhandene Codex-Karten direkt bearbeiten
- aktiver Typfilter wird für neue Einträge vorausgewählt

### ↔ Beziehungen

- + Wissenseintrag ergänzt
- Beziehungsknoten per Doppelklick direkt bearbeiten
- Beziehungen und ihre Knoten können im selben Bereich aufgebaut werden

### 🧩 Gemeinsame Wissensbasis

Die internen Wissenseinträge bleiben gemeinsame Datenbasis für:

- Figurenbogen
- Weltkarte
- Codex
- Beziehungen
- Story Graph
- Universum
- Kontinuität
- Manuskript-Verknüpfungen

Der technische Container bleibt intern bestehen, ist aber nicht mehr
die sichtbare Verwaltungsstelle.

### 🔄 Speichern und Löschen

Wissenseinträge aktualisieren nach Speichern oder Löschen
direkt den aktuell geöffneten Fachbereich.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Figuren direkt im Figurenbogen anlegbar/bearbeitbar
- Orte direkt in der Weltkarte anlegbar/bearbeitbar
- Codex kann Wissenseinträge direkt anlegen/bearbeiten
- Beziehungen können Wissenseinträge direkt anlegen
- aktueller Fachbereich wird nach Speichern/Löschen aktualisiert
- keine doppelten HTML-IDs

## v0.80.23

### 🔎 Kategorieauswahl bei „Neue Idee“ klar getrennt

Die bisherige Kategorieauswahl zeigte gleichzeitig die aktuelle
Kategorie und diente als Suchfeld.

Dadurch war nicht klar erkennbar, dass man dort suchen kann.

### Jetzt gibt es zwei getrennte Elemente

1. Ausgewählte Kategorie
   - zeigt nur die aktuell gewählte Kategorie
   - Klick öffnet die Auswahl

2. Eigenes Suchfeld
   - startet leer
   - durchsucht sofort alle Kategorien
   - sucht in Kategoriename und vollständigem Pfad

### Vorschlagsliste

Jeder Treffer zeigt:

- Kategoriename
- vollständigen Hierarchiepfad

Die Liste bleibt scrollbar.

### ⌨ Tastatur

- Pfeil hoch/runter → Treffer wechseln
- Enter → übernehmen
- Escape → schließen

### Auswahl beim Öffnen

Die aktuelle Kategorie bleibt vorausgewählt.

Sie wird aber nicht mehr in das Suchfeld geschrieben.

Damit ist jetzt klar:

> aktuelle Kategorie = Auswahl  
> Suchfeld = Suche

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- separates Suchfeld vorhanden
- separate Kategorieauswahl vorhanden
- Speichern verwendet die tatsächlich ausgewählte Kategorie
- Suche über Name und Pfad
- keine doppelten HTML-IDs
- keine Datenmigration notwendig

## v0.80.22

### ✦ Wissen & Welt wird zur gemeinsamen Wissensbasis

Die Buchzuordnung sitzt jetzt auf der richtigen Ebene:
bei Wissen & Welt.

In der Navigation von Wissen & Welt gibt es:

> ⚙ Bücherzuordnung

Dort werden per Mehrfachauswahl alle Bücher festgelegt,
die dieselbe Wissensbasis verwenden.

Das gilt gemeinsam für:

- Charaktere
- Orte
- Weltwissen / Lore
- Codex
- Figurenbogen
- Beziehungen
- Weltkarte
- Story Graph
- weitere Werkzeuge, die auf diesen Wissensdaten aufbauen

Änderungen an der gemeinsamen Wissensbasis stehen dadurch
in allen ausgewählten Büchern zur Verfügung.

### 📎 Anhang neu definiert

Der sichtbare Bereich Anhang enthält keine vorbereiteten
Story-Bibel-Reiter mehr.

Entfernt wurden dort:

- Charaktere
- Orte
- Welt & Lore
- Notizen als vorgegebene Reiter
- World Building Studio / Modulsteuerung
- zweite Freigabe-Schaltfläche

Die zugrunde liegenden Wissensdaten bleiben vollständig erhalten.

### ＋ Anhang nur noch für eigene Reiter

Der Anhang startet leer und dient ausschließlich frei
angelegten Zusatzbereichen.

Beispiele:

- Soundtrack
- Zitate
- Moodboard
- Bonusmaterial
- Leseliste
- eigene Sammlungen

Eigene Reiter behalten:

- frei definierbare Templates
- Felder
- Ordner
- Unterordner
- Bilder
- Notizen / Inhalte

### 🧭 Weniger Doppelstrukturen

Die Trennung ist jetzt:

- Wissen & Welt = gemeinsame strukturierte Wissensbasis
- Anhang = selbst definierte Zusatzbereiche

Charaktere, Orte und Weltwissen müssen nicht mehr in einem
zweiten sichtbaren System gepflegt werden.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- zentrale Bücherzuordnung in Wissen & Welt gebunden
- Anhang zeigt nur benutzerdefinierte Reiter
- bestehende System-/Worldbuilding-Daten werden nicht gelöscht
- bestehende eigene Reiter bleiben sichtbar
- Einträge übernehmen weiterhin die Buchzuordnung der Wissensbasis
- keine doppelten HTML-IDs

## v0.80.21

### 🏠 Frischer Start und F5 jetzt getrennt

Das Startverhalten wurde klar getrennt:

- index.html neu öffnen → Dashboard
- F5 / Browser neu laden → letzte Arbeitsposition wiederherstellen

Damit beeinflusst der gespeicherte F5-Kontext nicht mehr den normalen Start der Suite.

### Technisch

Beim Start wird die Browser-Navigation geprüft:

- navigate → Dashboard
- reload → Resume-Kontext wiederherstellen
- ältere Browser verwenden performance.navigation als Fallback

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- frischer Start verwendet showDashboard()
- F5 verwendet weiterhin restoreResumeContext()
- bestehende Buch-/Projekt-/Manuskript-Kontexte bleiben beim Reload erhalten
- keine Datenmigration notwendig

## v0.80.20

### 🛠 Story-Bibel – „Charaktere anlegen“ repariert

Der Button zum Anlegen eines Charakters war nach der Vereinfachung
der Buchfreigabe tatsächlich kaputt.

Ursache:

- die Freigabe pro Eintrag war aus der Oberfläche entfernt worden
- openAppendixEntryDialog() griff aber weiterhin auf die inzwischen
  nicht mehr vorhandenen Elemente appendixEntryBooks und
  appendixEntryScope zu
- dadurch brach das Öffnen des Eintragsdialogs mit einem
  Laufzeitfehler ab

Die veralteten Referenzen wurden vollständig entfernt.

Betroffen waren potenziell:

- Charaktere
- Orte
- Welt & Lore
- Notizen
- eigene Reiter / Grundtypen

### ⚙ Story-Bibel-Freigabe ebenfalls repariert

Eine alte Referenz auf den bereits entfernten deleteAppendixBtn
wurde ebenfalls entfernt.

### 🔎 Gründlicher Aktions-Audit

Nach der Reparatur geprüft:

- keine statisch referenzierten HTML-IDs ohne vorhandenes Element
- keine direkt gebundenen Event-Handler ohne implementierte Funktion
- keine statischen data-*-Aktionsattribute ohne JavaScript-Verwendung
- keine doppelten HTML-IDs
- JavaScript-Syntax gültig

### 🛡 Story-Bibel-Bindings robuster

Die wichtigsten dynamischen Story-Bibel-Buttons werden jetzt
null-sicher gebunden.

Ein fehlendes optionales Bedienelement soll dadurch nicht mehr
nachfolgende Aktionen mitreißen.

### 🧪 Systemtest erweitert

Der vollständige Systemtest prüft jetzt zusätzlich:

- Story-Bibel-Dialoge
- Story-Bibel-Formulare
- Kernfunktionen zum Anlegen und Speichern

### 🗂 Kleine UX-Korrektur

Im Eintragsdialog heißt die leere Ordnerauswahl jetzt:

— Kein Ordner —

statt „Reiter-Wurzel“.

## v0.80.18

### 🔗 Buchzuordnung nur noch für die gesamte Story-Bibel

Die Auswahl „Verfügbar in“ wurde aus jedem einzelnen
Story-Bibel-Eintrag entfernt.

Charaktere, Orte, Weltwissen und Notizen erben jetzt automatisch
die Buchzuordnung ihrer Story-Bibel.

### 📚 Direkte Mehrfachauswahl

Unter „Freigabe“ der Story-Bibel wird nur noch festgelegt,
zu welchen konkreten Büchern sie gehört.

- direkte Mehrfachauswahl der Bücher
- mindestens ein Buch erforderlich
- keine Modi „Dieses Buch“, „Ganze Reihe“ oder „Universum“ mehr
- Änderung der Story-Bibel-Zuordnung synchronisiert vorhandene
  Einträge automatisch

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- keine Eintrags-Freigabe mehr im Eintragsdialog
- Buch-Mehrfachauswahl auf Story-Bibel-Ebene
- bestehende Einträge werden bei Änderungen synchronisiert
- Ordner, Reiter und Inhalte bleiben erhalten

## v0.80.17

### 📁 Story-Bibel – Ordnernavigation vereinfacht

Die bisher sichtbare „Wurzelebene“ wurde entfernt.

Jetzt gilt:

- ein neuer Reiter startet ohne vorgegebenen Ordner
- links steht zunächst nur „Ordner“ und „+ Ordner“
- solange kein Ordner angelegt wurde, erscheint nur ein dezenter Hinweis
- Einträge ohne Ordner werden direkt im Reiter angezeigt
- erst selbst angelegte Ordner erscheinen in der Liste

### ↩ Rückkehr aus einem Ordner

Wenn ein Ordner geöffnet ist, erscheint:

> ← Alle [Reitername]

Damit gelangt man wieder zur normalen Ansicht des jeweiligen Reiters.

### 🔤 Lesbarkeit verbessert

Vergrößert wurden unter anderem:

- Überschrift „Ordner“
- Ordnernamen
- Eintragszahlen
- Ordneraktionen
- Breadcrumb
- Bereichs-/Template-Hinweise

Die Ordnerspalte wurde außerdem etwas breiter.

### 🧭 Reiterbezogene Ordner

Ordner auf oberster Ebene werden sauber dem jeweiligen Story-Bibel-Reiter zugeordnet.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- bestehende Ordner und Unterordner bleiben erhalten
- Einträge ohne Ordner bleiben erhalten
- Umbenennen/Löschen bestehender Ordner bleibt möglich
- Unterordner bleiben möglich
- keine Datenmigration notwendig

## v0.80.16

### 📚 Bucheinstieg öffnet immer das Buch-Dashboard

Wenn ein Buch bewusst geöffnet wird, startet es jetzt immer auf seiner eigenen Übersicht.

Das gilt für:

- Klick auf ein Buch im Bücherregal
- Klick auf ein Buch auf dem Suite-Dashboard
- Wechsel auf ein anderes Buch über die Buchauswahl
- automatischer Einstieg in das einzige Buch eines Romanprojekts

Nicht mehr verwendet wird beim normalen Bucheinstieg:

- der zuletzt geöffnete Buch-Reiter
- der zuletzt gespeicherte Workflow-Reiter
- eine automatisch aus dem Buchstatus gewählte Unteransicht

Damit ist der Einstieg eindeutig:

> Buch anklicken → Buch-Dashboard

### 🧭 Gezielte Tiefenlinks bleiben erhalten

Direkte Sprünge aus anderen Funktionen bleiben weiterhin möglich.

Beispiele:

- Suchtreffer → konkrete Manuskriptszene
- Timeline → Ereignis
- Weltkarte → Ort
- Story Graph → verknüpfter Inhalt
- Codex → Eintrag

### ↻ F5 bleibt davon getrennt

- Buch bewusst neu öffnen → Dashboard
- F5 innerhalb eines bereits geöffneten Buchbereichs → möglichst dort bleiben

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Bücherregal verwendet Dashboard-Einstieg
- Dashboard-Buchkarten verwenden denselben Einstieg
- Buchwechsel setzt Workflow auf Dashboard
- Tiefenlinks bleiben erhalten
- F5-Restore bleibt erhalten

## v0.80.15

### 📖 Eine Story-Bibel pro Buch

Die zusätzliche linke Anhang-Verwaltung wurde entfernt.

Für ein Buch gibt es jetzt genau eine sichtbare Story-Bibel. Die eigentliche Organisation erfolgt darin über:

- Reiter
- eigene Reiter
- Ordner
- Unterordner
- Story-Bibel-/Worldbuilding-Einträge

### 🧭 Oberfläche vereinfacht

Entfernt wurden:

- linke Liste mehrerer Anhänge
- „+ Neuer Anhang“
- „Bestehenden Anhang verknüpfen“
- „Verknüpfung lösen“

Der Story-Bibel-Arbeitsbereich nutzt jetzt die volle Breite.

Oben bleiben:

- Module
- Eigener Reiter
- Freigabe

### 🔗 Freigabe bleibt erhalten

Die Story-Bibel kann weiterhin gelten für:

- dieses Buch
- mehrere Bücher
- ganze Reihe
- Universum

Einzelne Worldbuilding-Einträge behalten zusätzlich ihre eigene Freigabelogik.

### 🔄 Bestehende mehrere Anhänge

Falls ältere Daten für ein Buch mehrere Anhänge enthalten, werden diese beim ersten Öffnen automatisch zu einer gemeinsamen Story-Bibel zusammengeführt.

Übernommen werden:

- Einträge
- eigene Reiter
- Systemreiter
- Ordner
- Unterordner
- Eintrags-Freigaben

### 🧩 Interne Kompatibilität

Der technische Appendix-Datensatz bleibt intern als Container bestehen.

In der Oberfläche ist diese technische Ebene nicht mehr als eigene Verwaltung sichtbar.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Story-Bibel wird bei fehlendem Container automatisch angelegt
- mehrere ältere Container werden konsolidiert
- linke Mehrfach-Anhang-Navigation entfernt
- Reiter-/Ordner-/Eintragslogik bleibt bestehen
- Scope-/Freigabesystem bleibt bestehen
- keine doppelten HTML-IDs

## v0.80.14

### 🔎 Suche wird bei bewusster Navigation verlassen

Im Ideenarchiv bleibt eine laufende Suche jetzt nicht mehr aktiv, wenn anschließend links bewusst in eine Kategorie gewechselt wird.

Das gilt für:

- Klick auf eine Kategorie im Kategorienbaum
- Öffnen einer Kategorie per Tastatur
- Wechsel über ein Schnellregister
- Wechsel über die Kategorie-Suche

Beim Wechsel wird:

- der Suchbegriff geleert
- die Suchansicht beendet
- die Trefferanzeige zurückgesetzt
- die gewählte Kategorie normal geöffnet
- eine eventuell offene globale Suchpalette geschlossen

Damit verhält sich die Navigation wieder erwartbar: Wer nach einer Suche links eine Kategorie auswählt, landet direkt in dieser Kategorie und sieht nicht weiterhin die alten Suchtreffer.

## v0.80.13

### 🔗 Gemeinsames Freigabe-/Scope-System für Anhänge & Worldbuilding

Anhänge und strukturierte Story-Bibel-/Worldbuilding-Einträge bleiben unterschiedliche Inhaltstypen, verwenden jetzt aber dieselbe Logik dafür, wo sie verfügbar sind.

### 📎 Anhänge

Beim Anlegen oder Bearbeiten eines Anhangs:

- Dieses Buch
- Mehrere Bücher
- Ganze Reihe
- Universum

Standard:
- Dieses Buch

### 🌍 Worldbuilding-/Story-Bibel-Einträge

Dieselbe Freigabelogik gilt jetzt auch für:

- Charaktere
- Orte
- Weltwissen
- weitere strukturierte Einträge

Ein Charakter muss damit nicht pro Band neu angelegt werden.

### 🧬 Gemeinsame Akte statt Dubletten

Ein Eintrag bleibt ein einzelner Datensatz und kann mehreren Büchern, einer Reihe oder dem Universum zur Verfügung stehen.

Bandabhängige Änderungen bleiben separat über:

- Kontinuitätsstände
- Character Arc
- Living Canon / Bandvarianten

abbildbar.

### 📚 Reihenlogik

- „Ganze Reihe“ nutzt den Reihenbezug des Ausgangsbuchs.
- Ohne eigenen Reihennamen dient das Romanprojekt als gemeinsamer Kontext.

### 🌌 Universum

- Eintrag bzw. Anhang wird in allen vorhandenen Büchern verfügbar.

### 🧩 Unterschiedliche Reichweite möglich

Ein einzelner Eintrag darf weiter geteilt sein als sein Anhang.

Der Anhang wird dann in den betreffenden Büchern automatisch als Container sichtbar, ohne dass alle übrigen Inhalte mitgeteilt werden.

### 🔄 Bestehende Daten

- bestehende Anhänge bleiben erhalten
- bestehende Mehrfachzuweisungen bleiben erhalten
- bestehendes Reihen-/Universumswissen wird übernommen
- keine destruktive Migration

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Scope-Modell für Anhänge ergänzt
- Scope-Modell für Story-Bibel-Einträge ergänzt
- bestehende bookIds bleiben kompatibel
- dynamische Reihen-/Universums-Sichtbarkeit ergänzt
- Container-Sichtbarkeit bei weiter geteilten Einträgen ergänzt
- keine doppelten HTML-IDs

## v0.80.12

### 🌳 Kategorien per Drag & Drop sortieren und verschachteln

- Kategorien können direkt per Maus neu sortiert werden.
- Drop oberhalb einer Kategorie → davor einsortieren.
- Drop mittig → als Unterkategorie verschachteln.
- Drop unterhalb → dahinter einsortieren.
- Verschieben funktioniert auch über Hierarchieebenen hinweg.
- Alte und neue Reihenfolge werden automatisch normalisiert.
- Zielordner wird beim Verschachteln automatisch aufgeklappt.
- zyklische oder ungültige Verschachtelungen werden verhindert.

### 🎯 Visuelles Drop-Feedback

- Linie oben → davor
- grünliche Hervorhebung → darunter
- Linie unten → dahinter
- gezogene Kategorie leicht transparent

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- bestehendes Verschieben auf Hauptebene erhalten.
- Ideen-Drag-&-Drop bleibt unberührt.
- Reihenfolge persistent gespeichert.
- keine Datenmigration notwendig.

## v0.80.11

### 🔎 Suchbare Kategorieauswahl bei „Neue Idee“

- Lange native Kategorienliste ersetzt.
- Kategorie direkt eintippen.
- Treffer werden während der Eingabe vervollständigt.
- Suche über Kategorienamen und vollständigen Kategoriepfad.
- Treffer zeigen Name + Hierarchiepfad.
- sinnvolle Trefferreihenfolge.
- Vorschlagsliste scrollbar.
- maximal 80 sichtbare Treffer.

### ⌨ Tastaturbedienung

- Pfeil runter / hoch → navigieren
- Enter → übernehmen
- Escape → schließen
- Dropdown-Pfeil → alle Kategorien anzeigen

### 🧭 Eindeutige Auswahl

- vollständiger Kategoriepfad nach Auswahl sichtbar.
- eindeutige exakte Texteingabe wird akzeptiert.
- unbekannte oder mehrdeutige Eingabe wird nicht versehentlich einer alten Kategorie zugeordnet.
- stattdessen Hinweis zur Auswahl eines Vorschlags.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- bestehende Ideen-Speicherlogik erhalten.
- Kategoriehierarchie vollständig durchsuchbar.
- keine Datenmigration notwendig.

## v0.80.10

### ↻ F5 / Reload – letzte Unterseite wiederherstellen

- Nach F5 wird nicht mehr grundsätzlich das Dashboard geöffnet.
- Gespeichert werden Hauptbereich und wichtiger Unterkontext.
- Wiederhergestellt werden nach Möglichkeit:
  - Ideenarchiv-Kategorie
  - geöffnetes Romanprojekt und Projektreiter
  - geöffnetes Buch
  - Buch-Workflow-Reiter
  - aktiver Manuskriptabschnitt
  - aktive Mindmap
  - wichtige Bereichsfilter
- Existiert ein gespeichertes Objekt nicht mehr, wird sicher auf einen passenden Hauptbereich zurückgefallen.

### 📥 Mehrfachimport – manuelle Auswahl gewinnt

- Automatisches Überspringen exakter Dubletten steuert nur noch die Vorauswahl.
- Das Häkchen jeder Vorschauzeile ist die endgültige Entscheidung.
- Manuell wieder angehakte Dubletten werden übernommen.
- Nicht angehakte Zeilen werden übersprungen.
- Ohne ausgewählte Zeilen wird nichts importiert.

### ⚠ Dublettenwarnung – Fundort beim Hover

- Kategoriepfad vorhandener exakter Dubletten wird beim Mouse-over angezeigt.
- Bei mehreren Treffern werden alle Fundorte angezeigt.
- Fundorte sind anklickbar und öffnen direkt die vorhandene Idee.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Reload-Kontext gespeichert.
- F5-Restore integriert.
- Mehrfachimport respektiert manuelle Häkchen.
- Dubletten-Fundorte verknüpft.
- keine Datenmigration notwendig.

## v0.80.9

### ✒ Lesbarere Ideenzettel

- Schreibmaschinen-/Monospace-Schrift der Ideenzettel entfernt.
- Ideeninhalte verwenden jetzt Georgia.
- Schriftgröße und Zeilenhöhe für längere Notizen verbessert.
- Metadaten und Bedienelemente bleiben in der klaren UI-Schrift.
- Papier-, Tape- und Zettelkastenoptik bleiben erhalten.

### 🌳 Kategorien schneller ein- und ausklappen

- Normaler Klick auf eine Kategorie klappt vorhandene Unterkategorien direkt auf.
- Ein weiterer Klick klappt sie wieder zu.
- Die angeklickte Kategorie bleibt gleichzeitig als Filter ausgewählt.
- Rechtsklick behält das Kategorien-Kontextmenü.
- Drag & Drop und •••-Bearbeiten bleiben unverändert.
- Enter/Leertaste verhalten sich entsprechend.
- Pfeiltasten rechts/links öffnen bzw. schließen weiterhin gezielt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Kategorie-Drag-&-Drop erhalten.
- Rechtsklick-Handler erhalten.
- Masonry-Layout bleibt erhalten.
- keine Datenmigration notwendig.

## v0.80.8

### 🛠 Reliability Rebuild – Kernfunktionen repariert

### 🔥 Gefundene eigentliche Ursache

Im Mindmap-Code fehlten:

- openMindmapEdgeDialog
- saveMindmapEdge
- deleteMindmapEdge

deleteMindmapEdge wurde beim Start direkt als Handler referenziert.

Dadurch brach bindEvents() mit einem ReferenceError ab.

Alle danach liegenden UI-Bindings wurden nicht mehr gesetzt.

Betroffen waren unter anderem:

- neue Mindmap
- Mindmap bearbeiten
- Namenslisten
- Statistiken
- Inspiration
- Recherche
- Romanprojekte
- Romanideen
- weitere Formulare und Filter

### 🕸 Mindmap-Verbindungen vollständig implementiert

- Verbindungsdialog
- Von-/Zu-Knoten
- Beschriftung
- Linienstil
- Speichern
- Bearbeiten
- Löschen
- Validierung

### 🧱 Kernbindings entkoppelt

Unabhängige Bindings für:

- Bücher
- Romanideen
- Romanprojekte
- Namenslisten
- Recherche
- Inspiration
- Mindmaps
- Statistiken

### 🛡 Doppelte Ausfallsicherung

Kernaktionen besitzen jetzt:

1. direkte modulare Bindings
2. zentrale Event-Delegation als Sicherheitsnetz

### 🧪 Vollständiger Systemtest

Neu unter Einstellungen:

- Kernbuttons
- Kernformulare
- Hauptnavigation
- Funktionskern
- Render-Smoke-Test
- echter IndexedDB-Schreib-/Löschtest über alle Suite-Stores

### 🧭 Weitere reparierte Laufzeitlücken

- universeBookIdsForTimeline ergänzt
- renderWritingView → renderWriting korrigiert
- docxPageBreak ergänzt

### ✓ Abschlussprüfung

- keine fehlenden Event-Handler
- keine fehlenden Kernfunktionen
- keine ungebundenen Formulare
- keine unreferenzierten statischen Buttons
- keine fehlenden UI-IDs
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- JavaScript-Syntax gültig

## v0.80.7

### 🗂 Ideenarchiv – versetztes Kartenlayout

- Ideenraster auf versetztes Masonry-Layout erweitert.
- Lange Ideen erzeugen keine großen Leerflächen mehr neben kurzen Karten.
- Nachfolgende Karten rücken in freie Bereiche nach.
- Listen- und Fokusansicht bleiben unverändert.
- Kartengrößen werden bei Änderungen automatisch neu berechnet.

### 🔗 Aktionslinks – zusätzliche Fallback-Navigation

Zusätzliche zentrale Absicherung für:

- Suite-Dashboard
- Buch-Dashboard
- Romanprojekt-Dashboard
- Workflow-Sprünge
- Romanideen
- Recherchekarten
- Inspirationskarten
- Romanprojektkarten
- Bücherregal
- Analyse → Manuskript

Die lokalen Click-Handler bleiben bestehen.

Zusätzlich gibt es jetzt eine globale Delegation als Sicherheitsnetz, damit ein Fehler in einem einzelnen Modul nicht mehr andere Aktionslinks wirkungslos macht.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Masonry-Berechnung integriert
- ResizeObserver abgesichert
- Suite-Dashboard-Aktionen abgesichert
- Buch-Dashboard-Aktionen abgesichert
- Projekt-Dashboard-Aktionen abgesichert
- dynamische Hauptkarten mit Fallback versehen
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionsdefinitionen

## v0.80.6

### 🔗 Navigation – echter Funktionsfix

- Hauptnavigation vollständig auf direkte zentrale Routing-Funktion umgestellt.
- Explizite Routen für:
  - Dashboard
  - Ideenarchiv
  - Romanideen
  - Bücher & Projekte
  - Namenslisten
  - Recherche
  - Inspiration
  - Mindmaps
  - Statistiken
  - Einstellungen
- Seitenleiste und Hamburger-Menü verwenden dieselbe Route.
- Logo verwendet dieselbe Dashboard-Route.
- Buttons zusätzlich direkt gebunden.
- globale Event-Delegation bleibt als Fallback erhalten.
- Navigation bleibt unabhängig von Fehlern anderer UI-Bindings verfügbar.
- eigener Navigation-Audit beim Start.

### 🧭 Seitenleiste

Außerhalb des Ideenarchivs:

- Suite-Menü wieder untereinander.
- nutzt die freie Seitenhöhe.
- keine zweispaltige Stauchung.
- keine unnötig abgeschnittenen Menüeinträge.

Im Ideenarchiv:

- kein zusätzliches Suite-Menü in der linken Spalte.
- linke Fläche gehört vollständig dem Ideenarchiv.
- Kategorien, Statistik, Papierkorb und Schnellregister bleiben erhalten.
- globale Navigation weiterhin über Hamburger, Logo und globale Suche.

### 🌳 Kategorien

Weiterhin enthalten:

- Rechtsklick → Kategorie ein-/ausklappen.
- Rechtsklick → gesamten Unterbaum ein-/ausklappen.
- Hover-Tooltip bei abgeschnittenem Kategorienamen.
- globale Ein-/Ausklappsteuerung.
- gespeicherte Standardtiefe.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- zehn Hauptbereiche besitzen explizite Routing-Fälle.
- zehn Hauptbereiche besitzen vorhandene Zielansichten.
- Navigation-Audit integriert.
- keine fehlenden statischen UI-Referenzen.
- keine doppelten HTML-IDs.
- keine doppelten Funktionen.

## v0.80.4

### 🌳 Kategorien-Kontextmenü & Lesbarkeit

- Rechtsklick auf Kategorien im Ideenarchiv erweitert.
- Kategorie direkt ein-/ausklappen.
- Gesamten Unterbaum ein-/ausklappen.
- Baumaktionen nur bei Kategorien mit Unterkategorien sichtbar.

### 🖱 Lange Kategorienamen

- Lange Namen bleiben sauber gekürzt.
- Vollständiger Name erscheint beim Hover, wenn der Text abgeschnitten ist.

### 🔎 Schriftgrößen korrigiert

Deutlich angehoben wurden insbesondere:

- Autoren-Suite-Logo
- linke Hauptnavigation
- Ideenarchiv-Kontextnavigation
- Kategoriennamen und Zähler
- Kategorien-Rechtsklickmenü
- Dashboard-Untertexte
- Buchkarten und Metadaten
- Schnellaktionen
- Buchnavigation und Subtabs
- Einstellungen
- Hilfstexte und Kicker

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Kategorie-Rechtsklickmenü erweitert
- Einzelkategorie ein-/ausklappen geprüft
- Unterbaum ein-/ausklappen integriert
- Tooltip für abgeschnittene Kategorienamen integriert
- Schriftgrößen korrigiert
- keine doppelten HTML-IDs
- keine fehlenden statischen UI-Referenzen

## v0.80.3

### 🧭 Navigation Hotfix & konstante Seitenleiste

- Hauptnavigation mit fester zentraler Routing-Tabelle neu abgesichert.
- Alle Hauptbereiche besitzen eine definierte Route.
- Seitenleiste und Hamburger-Menü verwenden dieselbe Navigation.
- Navigationsbuttons zusätzlich direkt gebunden.
- Globale Event-Delegation bleibt als Fallback bestehen.
- Logo/Home verwendet dieselbe Dashboard-Route.
- Aktiver Bereich wird in der Navigation hervorgehoben.
- Ideenarchiv ersetzt die Suite-Navigation nicht mehr.
- Suite-Navigation bleibt dauerhaft sichtbar.
- Ideenarchiv erhält darunter eine eigene Kontextnavigation.
- Kategorien, Statistik, Papierkorb und Schnellregister bleiben vollständig erhalten.
- Suite-Navigation kompakter gestaltet.
- JavaScript-Syntax geprüft.
- keine fehlenden statischen UI-Referenzen.
- keine doppelten HTML-IDs.
- keine doppelten Funktionsdefinitionen.

## v0.80.2

### 🎨 Schnellregister & Kategorienbaum

- Farbige Schnellregister können jetzt in den Einstellungen vollständig ein- oder ausgeblendet werden.
- Jedes der vier Register kann zusätzlich einzeln sichtbar oder unsichtbar geschaltet werden.
- Die Zuteilung der Register zu Kategorien kann jetzt direkt in den Einstellungen vorgenommen werden.
- Registerzuweisungen bleiben lokal gespeichert.
- Die bisherige Schnellzuweisung durch Klick auf ein leeres Register bleibt zusätzlich erhalten.

### 🌳 Kategorienbaum – globale Steuerung

Direkt über dem Kategorienbaum gibt es jetzt Schnellaktionen für:

- alle Kategorien einklappen
- alle Kategorien ausklappen
- gespeicherte Standardtiefe anwenden

Die individuelle Steuerung einzelner Kategorien bleibt bestehen.

### ⌨ Kategorien per Tastatur

Wenn eine Kategorie fokussiert ist:

- Pfeil rechts → aufklappen
- Pfeil links → einklappen
- Enter / Leertaste → Kategorie öffnen

### ⚙ Einstellungen

Neu steuerbar:

- Standardtiefe des Kategorienbaums
- alle Kategorien sofort einklappen
- alle Kategorien sofort ausklappen
- Standardtiefe sofort anwenden
- komplette Schnellregister-Leiste anzeigen / ausblenden
- Sichtbarkeit jedes einzelnen Registers
- Kategorie-Zuteilung für Rosa, Blau, Grün und Flieder

### 🔒 Verhalten

- Ausblenden eines Registers löscht seine Zuteilung nicht.
- Beim Wiedereinblenden ist die bisherige Zuteilung wieder vorhanden.
- Ungültige Registerzuweisungen werden weiterhin automatisch bereinigt.
- Bestehende Zuordnungen bleiben kompatibel.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Register-Sichtbarkeit persistent
- individuelle Register-Sichtbarkeit geprüft
- Register-Zuteilung über Einstellungen integriert
- globale Kategoriensteuerung integriert
- individuelle Kategoriensteuerung erhalten
- Tastatursteuerung ergänzt
- Standardtiefe erhalten
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionsdefinitionen

## v0.80.1

### 🧭 UX-/Navigationsrunde I

- Neues Suite-Dashboard als zentrale Startseite.
- Autoren-Suite-Logo führt jetzt immer zum Dashboard.
- Große helle Bottom-Navigation vollständig entfernt.
- Hauptnavigation über Hamburger-Menü, Suite-Sidebar, Dashboard und globale Suche.
- Navigation außerhalb des Ideenarchivs zentral und robuster gebunden.
- Schnellregister wieder eindeutig Rosa, Blau, Grün und Flieder.
- Neue Einstellung für die Standardtiefe des Ideenarchiv-Kategorienbaums.
- Standard: 2 sichtbare Hierarchiestufen.
- Buchnavigation in grobe Arbeitsbereiche gruppiert.
- Nur zum gewählten Bereich passende Unterfunktionen werden angezeigt.
- Dauerhafte große Workflow-Ribbon ausgeblendet.
- Manuskriptoberfläche ruhiger und kompakter.
- Einstellungen um echte UX-Konfiguration erweitert.
- Responsive Navigation nachgezogen.
- bestehende Buchmodule und Funktionen bleiben erhalten.
- JavaScript-Syntax geprüft.
- keine fehlenden statischen UI-Referenzen.
- keine doppelten HTML-IDs.
- keine doppelten Funktionsdefinitionen.

## v0.80.0

### 🔬 Feature-Parity & Gap Audit

Systematischer Vergleich mit:

- Papyrus Autor
- Scrivener
- Plottr
- Campfire
- Novelcrafter
- bibisco

### 📊 Audit-Matrix

38 konkrete Funktionsfelder in neun Bereichen geprüft:

- Schreiben & Editor
- Planung & Plotting
- Worldbuilding
- Codex & Wissensnetz
- Analyse & Kontinuität
- Überarbeitung
- Compile & Publishing
- Serien & Universen
- UX, Portabilität & Reliability

Bewertung:

- ✦ USP
- ✓ stark / Parity
- ◐ teilweise
- ! echte Lücke

### ✦ Eigene Stärken

Besonders hervorgehoben:

- Story Growth
- Living Knowledge
- kontextuelles Arbeiten aus dem Manuskript
- Continuity Intelligence
- Living Canon
- Reliability / Datenintegrität

### ! Wichtigste verbleibende Lücken

- Rechtschreibung / Grammatik / Thesaurus
- Stilanalyse & Lesbarkeit
- DOCX-/Markdown-/HTML-Manuskriptimport
- Cloud-Sync / Kollaboration
- Mobile / Multi-Device

### ◐ Gezielter Ausbau

- Plot-/Genre-Template-Bibliothek
- frei konfigurierbarer Outliner
- Familienstammbaum
- Printsatz / Buchdesigner

### 🧭 Gap-Closing-Roadmap

- v0.81 – Language & Style Studio
- v0.82 – Import & Migration Studio
- v0.83 – Navigator & Outliner 3.0
- v0.84 – Template Library 2.0
- v0.85 – Publishing Polish
- v0.86 – Specialized Relationship Views
- v0.87 – Portability Decision

### 🧬 Ergebnis

Es fehlt kein weiterer grundlegender Architekturblock für:

- Story-Planung
- Worldbuilding
- Wissensnetz
- Kontinuität
- Revision
- Publishing
- Serienverwaltung

Die nächste Phase ist gezieltes Gap Closing statt allgemeinem Feature-Ausbau.

### ✓ Technische Prüfung

- Audit-Bereich integriert
- 38 Audit-Felder hinterlegt
- neun Funktionsbereiche geprüft
- Gap-Priorisierung integriert
- Folge-Roadmap integriert
- Workflow-Routing erweitert
- Audit-Panel responsiv
- JavaScript-Syntax geprüft
- 24 Workflow-Bereiche / 24 Workflow-Buttons
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare

## v0.79.0

### 🧩 Integration & Workflow 2.0

Diese Version fügt bewusst kein weiteres riesiges Fachmodul hinzu.

Stattdessen wurden die inzwischen vielen Bereiche der Suite enger zu einem gemeinsamen Arbeitsfluss verbunden.

Ziel:

> Weniger „In welchem Modul war das noch?“
> Mehr „Ich bin gerade hier – was kann ich von hier aus sinnvoll tun?“

### 🧩 Neuer Arbeitskontext im Manuskript

Der Manuskript-Inspector besitzt jetzt einen eigenen Reiter:

- 🧩 Kontext

Direkt sichtbar:

- Codex-/Story-Bibel-Verknüpfungen
- Handlungsfäden
- Ideen
- offene Revisionen
- weitere globale Verknüpfungen

### ＋ Aus dem Kontext heraus anlegen

Direkt aus dem aktuellen Manuskriptabschnitt:

- Story-Bibel-Akte
- Handlungsfaden
- Timeline-Ereignis
- Idee
- Recherche
- Überarbeitungsaufgabe

Direkte Sprünge zu:

- Plotting
- Kontinuität
- Story Graph
- Manuscript Analysis

### 📎 Story-Bibel aus einer Szene

Neue Akten aus dem Arbeitskontext werden nach dem Speichern automatisch mit dem Ausgangsabschnitt verknüpft.

Es entsteht keine zweite Kontextkopie.

Bei Abbruch wird die vorbereitete Verknüpfung verworfen.

### 🧵 Handlungsfaden aus einer Szene

Neue Fäden erhalten direkt:

- aktuelles Buch
- aktuellen Manuskriptabschnitt als erste Station

### 💡 Gedanken & Recherche

Die Schnellablage wird in den gemeinsamen Kontextworkflow eingebunden.

Neue Ideen und Rechercheeinträge werden direkt mit dem aktuellen Abschnitt verknüpft.

### ✦ Revision aus Kontext

Überarbeitungsaufgaben können unmittelbar am aktuellen Abschnitt angelegt werden.

### ⌘ „Was hängt hier dran?“

Der Arbeitskontext zeigt globale Verknüpfungen des aktuellen Manuskriptabschnitts.

Von dort kann direkt zu verbundenen Objekten gesprungen werden.

### 🖱 Rechtsklick im Manuscript Navigator

Rechtsklick auf:

- Teil
- Kapitel
- Szene

öffnet den Arbeitskontext.

Drag & Drop, Split View und normaler Klick bleiben erhalten.

### ⌨ Tastatur

Neuer Shortcut:

- Strg/Cmd + Shift + J → Arbeitskontext

### ⌕ Command Palette

Neue kontextabhängige Schnellaktionen:

- Arbeitskontext öffnen
- Buch-Integration prüfen

### 🧭 Workflow-Ribbon vervollständigt

Eigene Workflow-Texte jetzt auch für:

- Plotting
- Szenen
- Story Growth
- Continuity
- Calendar
- World Map
- Character Arc
- Story Graph
- Analysis
- Compile
- Codex

### 🏠 Integration auf dem Buch-Dashboard

Neuer Bereich:

- Integration & Workflow

Er zeigt kontextabhängige nächste Übergänge.

Beispiele:

- Ideen → Planung
- Planung → Szenen
- Szenen → Manuskript
- Manuskript → Story-Bibel
- Szenen → Handlungsfäden
- Revision → Revision Studio
- fertiges Buch → Compile

### ✓ Technischer Integritätscheck

Geprüft werden unter anderem:

#### Manuskript

- tote Ideenverknüpfungen
- tote Story-Bibel-Verknüpfungen
- tote Rechercheverknüpfungen
- tote Inspirationsverknüpfungen

#### Handlungsfäden

- Beats ohne existierenden Manuskriptabschnitt
- gelöschte Story-Bibel-Referenzen

#### Timeline

- gelöschte Story-Bibel-Referenzen

#### Buchbeziehungen

- Beziehungen zu nicht mehr vorhandenen Büchern

#### Compile

- Regeln für gelöschte Manuskriptabschnitte

#### Revision

- Snapshot-Zuordnungen zu nicht mehr vorhandenen Dokumenten oder Versionen

### 🧹 Sichere Bereinigung

Bereinigt werden ausschließlich tote technische Referenzen.

Nicht gelöscht werden:

- Manuskript
- Ideen
- Story-Bibel
- Recherche
- Snapshots

Vorher erfolgt eine Rückfrage.

### 🔧 Workflow-Routing

- Compile in fehlender Workflow-Whitelist nachgetragen.
- Initiale Panel-Steuerung für Analysis und Compile nachgezogen.

### 🔗 Einheitliche Navigation

Arbeitskontext, globale Suche, Command Palette und Living Knowledge verwenden stärker dieselbe Navigationslogik.

### 🧬 Produktprinzip

Ein Objekt ist der Kontext.

Von einer Szene aus kann ich:

- Wissen anlegen
- einen Faden eröffnen
- Revision markieren
- recherchieren
- Chronologie ergänzen
- in Plotting oder Analyse springen

ohne erst das passende Modul suchen zu müssen.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Inspector-Kontext geprüft
- Kontext-Routing geprüft
- Story-Bibel-Anlage aus Manuskriptkontext geprüft
- automatische Kontextverknüpfung geprüft
- Abbruch abgesichert
- Handlungsfaden mit erster Manuskriptstation geprüft
- Schnellablage eingebunden
- Revision aus Kontext integriert
- globale Verknüpfungsnavigation integriert
- Navigator-Rechtsklick integriert
- Tastaturshortcut integriert
- Command Palette erweitert
- Dashboard-Workflowempfehlungen integriert
- Integritätscheck integriert
- sichere Dead-Link-Bereinigung integriert
- Compile-Workflow-Whitelist korrigiert
- Workflow-Ribbon vervollständigt
- 23 Workflow-Bereiche / 23 Workflow-Buttons
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare


## v0.78.0

### 🧬 Living Canon & Bandvarianten

Die in v0.77 vorbereiteten `bookVariants` sind jetzt ein vollständiges Arbeitswerkzeug.

Ziel:

> Eine gemeinsame kanonische Akte – aber nachvollziehbare Zustände je Band.

Dadurch müssen Figuren, Orte, Organisationen oder Gegenstände nicht pro Buch dupliziert werden.

### 📚 Bandzustände pro Story-Bibel-Akte

Für jede Story-Bibel-Akte können jetzt je Buch eigene Zustände hinterlegt werden.

Mögliche Angaben:

- Status / Zustand
- Rolle in diesem Band
- bandbezogene Kurzfassung
- Ort / Aufenthaltsort
- Wissensstand
- Beziehungen / Zugehörigkeiten
- freie Notizen

Die gemeinsame Hauptakte bleibt unverändert der kanonische Kern.

### 🧠 Vererbung statt Duplikation

Existiert für einen Band kein eigener Zustand, gilt weiterhin die gemeinsame Hauptakte.

Prinzip:

> gemeinsamer Kanon + nur die Abweichungen je Band

statt:

> dieselbe Figur mehrfach kopieren und getrennt pflegen.

### ✎ Bandzustand-Editor

Im Universumsbereich kann für jede Akte und jeden Band direkt ein Bandzustand angelegt oder bearbeitet werden.

Ein Bandzustand kann gelöscht werden, ohne die Hauptakte zu entfernen.

### 🕸 Living Canon Detailansicht

Für eine ausgewählte Akte wird pro Band angezeigt:

- Gültigkeit
- Bandzustand
- Status
- Rolle
- Ort
- Wissen
- Bandnotiz
- Figurenentwicklungsstand
- Kontinuitätswerte
- Beziehungen

### ↔ Veränderungen zwischen Bänden

Verglichen werden unter anderem:

- Status
- Rolle
- Ort
- Wissensstand

Veränderungen werden als Übergänge zwischen den Bänden sichtbar gemacht.

### 👤 Character Arc Integration

Vorhandene Character-Arc-Stationen werden direkt in den Bandzustand eingeblendet.

Damit gilt:

- Bandzustand = grober Zustand im Buch
- Character Arc = konkrete Entwicklung innerhalb des Buchs

### ⌘ Beziehungsintegration

Bandbezogene Beziehungen werden in der Living-Canon-Ansicht berücksichtigt.

### 📌 Kontinuitätsintegration

Gemeinsamer Kanon, Bandzustände und bestehende Kontinuitätswerte werden zusammengeführt.

### 🧭 Canon Intelligence

Neue Hinweise unter anderem für:

- abweichenden Aufenthaltsort zwischen Bandzustand und Kontinuitätswert
- mehrere gleichzeitige Kernänderungen zwischen zwei Bänden

Hinweise bleiben bewusst nicht blockierend.

### 🧠 Codex

Der Codex zeigt jetzt vorhandene Bandzustände des aktuell geöffneten Buchs.

Bandzustände können direkt aus dem Codex bearbeitet werden.

### 🔎 Suche

Bandvarianten werden durchsucht.

Zusätzlich durchsuchbar:

- Status
- Rolle
- Bandzusammenfassung
- Ort
- Wissensstand
- Beziehungen
- Bandnotizen

### 🌌 Universums-Matrix

Ein Klick auf eine Akte öffnet jetzt zunächst ihre Living-Canon-Ansicht.

Von dort kann weiterhin die vollständige kanonische Hauptakte geöffnet werden.

### 💾 Datenmodell

`bookVariants` wird jetzt aktiv genutzt.

Je Akte:

bookVariants
  └─ Buch-ID
       ├─ status
       ├─ role
       ├─ summary
       ├─ location
       ├─ knowledge
       ├─ relationships
       ├─ notes
       └─ updatedAt

### 🔒 Kompatibilität

Bestehende Akten ohne Bandvarianten funktionieren unverändert.

Fehlt eine Variante, gilt automatisch der gemeinsame Kanon.

Bestehende:

- Kontinuitätswerte
- Character Arcs
- Beziehungen
- Codex-Daten
- Buchzuordnungen
- Reihen-/Universums-Scope

bleiben erhalten.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Bandvarianten-Datenmodell geprüft
- Bandzustand-Editor integriert
- Speichern und Löschen geprüft
- Living-Canon-Detailansicht integriert
- Band-zu-Band-Übergänge integriert
- Character-Arc-Integration geprüft
- Beziehungsintegration geprüft
- Kontinuitätsintegration geprüft
- Canon-Intelligence-Hinweise integriert
- Codex-Bandzustände integriert
- Bandvarianten in Suche integriert
- Living-Knowledge-Matrix erweitert
- bestehende Daten bleiben kompatibel
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionsdefinitionen
- keine ungebundenen Buttons/Formulare

## v0.77.0

### ✦ Serien & Universen 2.0

- Universumsbereich zu einer echten Ebene oberhalb einzelner Bücher ausgebaut.
- Universumsname und Beschreibung ergänzt.
- Universumsweite Kanon-Notizen ergänzt.

### 📚 Reihenstruktur

Automatische Gruppierung nach:

- Reihe
- Bandnummer
- Buchstatus

Zusätzlich sichtbar:

- Wortzahl
- offene Fäden
- Kontinuitätsstände

Bücher ohne Reihe werden separat dargestellt.

### ↔ Buchbeziehungen

Weiterhin unterstützt:

- Vorgänger / Nachfolger
- Prequel
- Fortsetzung
- Spin-off
- Parallelgeschichte
- gleiches Universum
- freie Beziehung

### 🧠 Wissens-Scope

Story-Bibel-Akten besitzen jetzt:

- Buch / ausgewählte Bände
- Reihenwissen
- Universumswissen

Die konkrete Bandzuordnung bleibt zusätzlich erhalten.

### 🕸 Living-Knowledge-Matrix

Neue Übersicht:

Story-Bibel-Akten × Bücher

Damit wird sichtbar:

- welche Figuren in welchen Bänden gelten
- welche Orte geteilt werden
- welches Weltwissen universumsweit gilt
- welche Akten nur lokal verwendet werden

### 📜 Universums-Kanon

Eigene universumsweite Kanon-Notizen.

Geeignet für:

- Weltregeln
- Magiegesetze
- historische Fixpunkte
- politische Grundordnung
- Serienkonventionen
- unveränderliche Fakten

### ◷ Master-Timeline

Timeline-Ereignisse aller Bücher werden zu einer gemeinsamen Chronologie zusammengeführt.

### 🧵 Bandübergreifende Story-Arcs

Mehrbändige Handlungsfäden werden im Universumsbereich gebündelt.

Geeignet für:

- Foreshadowing
- Geheimnisse
- Konflikte
- Beziehungen
- Versprechen
- Hinweise

### 🧭 Serien-Kontinuität

Neue Prüfungen unter anderem für:

- widersprüchliche Kontinuitätswerte
- mehrfach vergebene Bandnummern
- aufgelöste Reihenfäden ohne Auflösungsstation

Hinweise bleiben bewusst ruhig und nicht blockierend.

### 📊 Universums-Dashboard

Zeigt:

- Bücher
- Reihen
- geteilte Akten
- Reihenfäden
- Kontinuitäts-Prüfstellen

### 🧠 Codex

Scopes jetzt sauberer getrennt:

- Buch
- Reihe
- Universum / Romanprojekt

### 💾 Datenmodell

Neu:

- universeStudio
- Universumsname
- Universumsbeschreibung
- Kanon-Notizen
- Universums-Einstellungen
- universeScope pro Story-Bibel-Akte
- vorbereitete bookVariants

### 🔒 Kompatibilität

Bestehende:

- Bücher
- Reihen
- Buchbeziehungen
- Story-Bibel-Akten
- Timeline-Ereignisse
- Handlungsfäden
- Kontinuitätsdaten

bleiben erhalten.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Universums-Datenmodell geprüft
- Migration bestehender Projekte geprüft
- Wissens-Scope integriert
- Reihen-Gruppierung geprüft
- Buchbeziehungen integriert
- Living-Knowledge-Matrix integriert
- Master-Timeline integriert
- Reihenfäden integriert
- Kanon-Notizen integriert
- bandübergreifende Kontinuitätsprüfung integriert
- doppelte Bandnummern werden erkannt
- Codex-Scope angepasst
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionsdefinitionen

## v0.76.0

### 📦 Compile & Publishing Studio 2.0

Der Compile-Bereich wurde deutlich erweitert.

### 🧩 Publikationsstruktur pro Profil

Für jedes Compile-Profil kann jetzt einzeln festgelegt werden, welche Manuskriptbestandteile tatsächlich veröffentlicht werden.

Pro Teil, Kapitel und Szene:

- im Export enthalten / ausgeschlossen
- eigener Exporttitel
- Manuskripttitel bleibt unverändert
- für Teile und Kapitel kann ein Seitenumbruch erzwungen werden

Damit können beispielsweise:

- Bonuskapitel nur in einer Sonderausgabe erscheinen
- interne Szenen aus der Veröffentlichung ausgeschlossen werden
- Arbeitsüberschriften intern erhalten bleiben
- schönere Kapitelüberschriften nur im Export verwendet werden

### Aa Exporttitel

Jeder Manuskriptbestandteil kann einen profilabhängigen Exporttitel erhalten.

Beispiel:

Interner Szenentitel:

`Konfrontation mit Ardan – Version 3`

Exporttitel:

`Der Schwur`

Die Änderung betrifft ausschließlich die Ausgabe.

### 👁 Erweiterte Compile-Vorschau

Neu ist eine echte Textvorschau der zusammengeführten Ausgabe.

Sie zeigt die ersten Zeilen in der Reihenfolge, in der Frontmatter, Manuskript und Backmatter zusammengeführt werden.

Die Vorschau kann direkt kopiert werden.

### 🎛 Format-Overrides

Ein Compile-Profil kann einzelne Abweichungen je Ausgabeformat besitzen.

Unterstützt werden:

- EPUB
- DOCX
- PDF
- HTML
- RTF
- Markdown
- TXT

Je Format können insbesondere überschrieben werden:

- Seitengröße
- Schrift-Grundtyp

Wenn keine Abweichung gesetzt ist, gilt der gemeinsame Profilstandard.

### ↵ Individuelle Seitenumbrüche

Teile und Kapitel können unabhängig von der allgemeinen Profileinstellung einen Seitenumbruch erzwingen.

### 🚫 Selektiver Export

Ausgeschlossene Manuskriptbestandteile werden vom Compile-Plan entfernt.

Ist ein übergeordnetes Kapitel ausgeschlossen, werden dessen Szenen ebenfalls nicht veröffentlicht.

Das eigentliche Manuskript bleibt vollständig erhalten.

### 📋 Exportverlauf

Das Compile Studio protokolliert lokal die letzten Exporte eines Buchs.

Gespeichert werden:

- Format
- Dateiname
- verwendetes Compile-Profil
- Wortzahl
- Zeitpunkt

Der Verlauf wird vom bestehenden Backup erfasst.

### 📖 EPUB

Berücksichtigt jetzt zusätzlich:

- selektive Publikationsstruktur
- Exporttitel
- profilabhängige Szenentitel
- Format-Overrides

### 📝 DOCX

Berücksichtigt zusätzlich:

- ausgeschlossene Kapitel/Szenen
- Exporttitel
- individuelle Seitenumbrüche
- Format-Overrides

### 📄 PDF

Berücksichtigt zusätzlich:

- Publikationsstruktur
- Exporttitel
- individuelle Seitenumbrüche
- formatabhängige Seitengröße und Schrift

### HTML / RTF / Markdown / TXT

Auch diese Formate greifen auf die erweiterte Compile-Struktur zurück.

### 💾 Datenmodell

Compile-Profile enthalten jetzt zusätzlich:

- `documentRules`
- `formatOverrides`

Das Compile Studio enthält zusätzlich:

- `exportHistory`

Bestehende v0.75.0-Profile werden automatisch ergänzt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Migration bestehender Compile-Profile geprüft
- selektive Dokumentregeln integriert
- Eltern-/Szenen-Ausschlusslogik integriert
- Exporttitel integriert
- individuelle Seitenumbrüche integriert
- Format-Overrides integriert
- Textvorschau integriert
- Exportverlauf integriert
- EPUB/DOCX/PDF/HTML/RTF/Markdown/TXT-Routing geprüft
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionsdefinitionen

## v0.75.0

### 📦 Compile & Publishing Studio

- Neuer eigener Buch-Reiter „Compile“.
- Gemeinsames Compile-System für alle Manuskriptausgaben.
- EPUB, DOCX, PDF, HTML, RTF, Markdown und TXT verwenden denselben Veröffentlichungsaufbau.

### 🗂 Exportprofile

Beliebig viele Profile pro Buch.

Profilabhängig:

- Cover
- Titelseite
- Impressum / Rechte
- Inhaltsverzeichnis
- Kapitelnummerierung
- Teilnummerierung
- Szenentitel
- Szenentrenner
- Kapitelbeginn
- Seitengröße
- Schrift
- Schriftgröße
- Zeilenhöhe
- Erstzeileneinzug

Profile können:

- dupliziert
- umbenannt
- gelöscht
- aus Vorlagen erzeugt

werden.

### ✦ Schnellstart-Profile

- Roman · Standard
- Manuskript · Lektorat
- E-Book · Kompakt

Alle anschließend vollständig anpassbar.

### ♻ Projektvorlagen

Eigene Compile-Profile können im Romanprojekt gespeichert und in anderen Büchern desselben Projekts wiederverwendet werden.

Projektvorlagen werden vom normalen Backup erfasst.

### ↤ Frontmatter

Eigene Bestandteile vor dem Manuskript:

- Widmung
- Epigraph / Motto
- Vorwort
- freier Bestandteil

Automatisch erzeugbar:

- Titelseite
- Impressum / Rechte
- Inhaltsverzeichnis

Bestandteile können:

- sortiert
- bearbeitet
- ausgeblendet
- gelöscht

werden.

### ↦ Backmatter

Unter anderem:

- Nachwort
- Danksagung
- Über die Autorin / den Autor
- Weitere Bücher
- freie Bestandteile

### 🪪 Titelseite

Automatisch aus Buchdaten:

- Titel
- Untertitel
- Autor:in
- Reihe
- Bandnummer

### © Rechte / Impressum

Automatisch aus:

- Rechteangabe
- Autor:in
- Verlag / Imprint
- ISBN
- Ausgabe

### ≡ Inhaltsverzeichnis

Optional sichtbares Inhaltsverzeichnis.

EPUB erhält unabhängig davon immer seine technische Navigation.

### # Nummerierung

Optional beim Compile erzeugt:

- Teil 1 …
- Kapitel 1 …

Die echten Manuskripttitel bleiben unverändert.

### 🎬 Szenentitel

Pro Profil:

- sichtbar
- verborgen

### ⁂ Szenentrenner

- * * *
- Linie
- nur Abstand

### 📄 Kapitelbeginn

- neue Seite
- fortlaufend

### 📐 Seitengröße

- A5 / Roman
- A4 / Manuskript
- US Letter

### Aa Satz

- Serif
- Sans Serif
- Schriftgröße
- Zeilenhöhe
- Erstzeileneinzug

### 👁 Compile-Vorschau

Zeigt strukturell:

- Titelseite
- Frontmatter
- Inhaltsverzeichnis
- Teile
- Kapitel
- Backmatter

Zusätzlich:

- Ausgabeabschnitte
- Compile-Wortzahl
- Frontmatter
- Backmatter
- Validierungsstatus

### ✓ Compile-Prüfung

Prüft unter anderem:

- Titel
- Manuskriptstruktur
- Autor:in
- Cover
- Rechteangaben

Optionale Lücken erzeugen Hinweise statt harter Fehler.

### 📖 EPUB 3

Jetzt über den gemeinsamen Compile-Plan.

Unterstützt:

- Cover
- Frontmatter
- Backmatter
- Titelseite
- Impressum
- sichtbares Inhaltsverzeichnis
- EPUB-Navigation
- Teile / Kapitel
- Szenentitel
- Szenentrenner
- Manuskriptbilder
- Metadaten
- Reihe / Band
- Sprache
- Verlag
- Rechte
- Kennung / ISBN

### 📝 DOCX

Übernimmt unter anderem:

- Compile-Reihenfolge
- Front-/Backmatter
- Seitenumbrüche
- Seitengröße
- Schrift
- Schriftgröße
- Zeilenhöhe
- Einzug
- Kapitelstruktur

### 📄 PDF

Übernimmt denselben Compile-Aufbau inklusive:

- Front-/Backmatter
- Kapitel
- Inhaltsverzeichnis
- Seitenformat
- Satz-Grundeinstellungen
- Szenentrenner

### ▤ HTML

Vollständige Compile-Struktur mit druckfähigem CSS.

### RTF / Markdown / TXT

Nutzen ebenfalls die identische Inhaltsreihenfolge.

### 📦 Exportzentrum

Zeigt jetzt:

- aktives Compile-Profil
- Frontmatter
- Backmatter
- Szenentitelstatus
- Compile-Prüfung

Direkter Sprung ins Compile Studio.

### 💾 Backup

Gesichert werden:

- Buchprofile
- Einstellungen
- Frontmatter
- Backmatter
- Projektvorlagen
- Reihenfolge
- Aktivierungsstatus

### 🧬 Manuskript bleibt unverändert

Compile verändert keine:

- Manuskripttitel
- Szenentitel
- Textinhalte
- interne Struktur
- Verknüpfungen

Alle Publishing-Anpassungen entstehen ausschließlich beim Export.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Compile-Routing geprüft
- Profil-Datenmodell geprüft
- Schnellstart-Profile geprüft
- Projektvorlagen geprüft
- Frontmatter-CRUD geprüft
- Backmatter-CRUD geprüft
- Reihenfolge geprüft
- Compile-Validierung geprüft
- Nummerierung geprüft
- EPUB umgestellt
- DOCX umgestellt
- PDF umgestellt
- HTML umgestellt
- RTF umgestellt
- Markdown umgestellt
- TXT umgestellt
- Exportzentrum integriert
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare

## v0.74.0

### 🔄 Revision Studio 2.0

- Bestehenden Überarbeitungsmodus zu einem vollständigen Revision Studio ausgebaut.
- Überarbeitung kann jetzt in getrennten Revisionsdurchgängen organisiert werden.
- Ausgangs- und Abschlussfassungen pro Durchgang.
- Snapshot-gegen-Snapshot-Vergleich ergänzt.

### 🧭 Revisionsdurchgänge

Beliebig viele Durchgänge pro Buch.

Je Durchgang:

- Name
- Schwerpunkt
- Ziel
- Notiz
- Status
- Start
- Abschluss
- Ausgangsfassung
- Abschlussfassung

Schwerpunkte:

- Plot & Struktur
- Figuren
- Logik & Kontinuität
- Stil & Sprache
- Pacing & Szenenlänge
- Recherche
- eigener Schwerpunkt

### ◇ Status

- Geplant
- Aktiv
- Abgeschlossen

### 🎯 Revisionsziele

Prominent sichtbar:

- Ziel
- Schwerpunkt
- Status
- Start
- Abschluss
- eigene Arbeitsnotiz

### 📸 Ausgangsfassung

Beim Start optional automatisch komplette Manuskriptbasis sichern.

- ein Snapshot pro Manuskriptabschnitt
- fixiert
- einem Durchgang zugeordnet
- dauerhaft erhalten

### ✓ Abschlussfassung

Beim Abschluss wird ebenfalls eine fixierte Fassung gespeichert.

Dadurch möglich:

- Anfang Durchgang ↔ Ende Durchgang
- Durchgang 1 ↔ Durchgang 2
- spätere Änderungen verfälschen ältere Revisionsvergleiche nicht

### ↔ Fassungsvergleich

Je Durchgang:

- Wörter am Start
- Wörter aktuell bzw. am Abschluss
- Wortdifferenz
- veränderte Abschnitte
- neue Absätze
- entfernte Absätze

Je Abschnitt:

- Start-Wortzahl
- Ziel-/Aktuell-Wortzahl
- Differenz
- Absatzänderungen

### 🔀 Snapshot ↔ Snapshot

Vergleich jetzt möglich zwischen:

- Snapshot ↔ aktuell
- Snapshot ↔ Snapshot
- Ausgangsfassung ↔ Abschlussfassung
- beliebigen Revisionsständen

### 📝 Überarbeitungsaufgaben

Überarbeitungsnotizen können einem Revisionsdurchgang zugeordnet werden.

Der aktive Durchgang wird automatisch vorausgewählt.

Bestehende Notizen bleiben erhalten.

### 🗂 Aufgabenfilter

- aktueller / ausgewählter Durchgang
- alle Aufgaben
- ohne Durchgang

### 📌 Kanban

Weiterhin:

- Offen
- In Arbeit
- Erledigt

### ✦ Durchgang auf Karten sichtbar

Revisionskarten zeigen zusätzlich:

- Durchgang
- Schwerpunkt
- direkten Vorher/Nachher-Vergleich, wenn eine Ausgangsfassung vorhanden ist

### 🧩 Revisionsplan

Je Durchgang sichtbar:

- Reihenfolge
- Schwerpunkt
- Status
- Aufgabenfortschritt
- Basisstände

Aktionen:

- ansehen
- bearbeiten
- aktivieren
- abschließen

### 📊 Fortschritt

Pro Durchgang:

- offen
- in Arbeit
- erledigt
- Gesamtzahl
- Fortschrittsprozent

### 🛡 Sicheres Abschließen

Bei offenen Aufgaben erfolgt eine Rückfrage.

Aufgaben werden nicht automatisch verändert.

### 🗑 Sicheres Löschen

Beim Löschen eines Durchgangs:

- Manuskript bleibt
- Aufgaben bleiben
- Snapshots bleiben
- nur Durchgangszuordnung wird entfernt

### 📊 Manuscript Analysis

Direkter Wechsel zwischen:

- Analyse
- Revision
- Manuskript

### 📸 Snapshot-System

Revisionsfassungen verwenden das bestehende vollständige Versioning-System.

### 💾 Backup

Automatisch enthalten:

- Durchgänge
- Ziele
- Status
- Aufgaben-Zuordnungen
- Ausgangsfassungen
- Abschlussfassungen
- Snapshots

### 🔒 Kompatibilität

- bestehende Revisionsnotizen bleiben erhalten
- alte Notizen erscheinen als „ohne Durchgang“
- bestehende Snapshots bleiben vollständig nutzbar

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Revisionsdurchgang-Modell geprüft
- Ausgangsfassungen geprüft
- Abschlussfassungen geprüft
- Snapshot-Zuordnung geprüft
- Aufgaben-Zuordnung geprüft
- Durchgangsfortschritt geprüft
- Aufgabenfilter geprüft
- Ausgang ↔ aktuell geprüft
- Ausgang ↔ Abschluss geprüft
- Snapshot ↔ Snapshot geprüft
- sicheres Abschließen geprüft
- sicheres Löschen geprüft
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare

## v0.73.0

### 📊 Manuscript Analysis Studio

- Neuer eigener Buch-Reiter „Analyse“.
- Strukturelle Manuskriptanalyse auf Basis vorhandener Daten.
- Keine Qualitätsbewertung oder vorgeschriebenen Idealwerte.

### ⌂ Analyse-Übersicht

Kennzahlen:

- Szenenanzahl
- Szenenwörter
- durchschnittliche Szenenlänge
- Szenenmedian
- Kapitelanzahl
- Dialog-Näherung

Direkte Übersichten für:

- POV
- Figuren
- Orte
- strukturelle Hinweise

### 📏 Szenenlängen

Je Szene:

- Titel
- Wortzahl
- POV
- Ort
- Status
- relative Länge

Beschreibende Einordnung anhand des eigenen Szenenmedians:

- leer
- kurz
- typisch
- lang

### ▤ Kapitelanalyse

Je Kapitel:

- Wortumfang
- enthaltene Szenen
- unterschiedliche POVs
- unterschiedliche Orte

### 👁 POV-Analyse

Je POV:

- Szenen
- Szenenanteil
- Wortumfang
- längste Abwesenheit

Szenen ohne POV bleiben sichtbar.

### 👤 Figurenpräsenz

Je Figur:

- Anzahl Auftritte
- Szenenanteil
- Wortumfang
- längste Pause zwischen Auftritten

### ⌖ Ortsverteilung

Je Schauplatz:

- Szenen
- Anteil
- Wörter
- längste Pause

### 🧩 Plotlinienanalyse

Je Plotlinie:

- Szenenzahl
- Verteilung über das Manuskript
- längste Pause
- Typ
- Status

Mit visuellem Sequenzband.

### 🧵 Handlungsfäden

Je Faden:

- Szenenbeats
- Positionen im Manuskript
- längste Pause
- Typ
- Status

### 〰 Pacing-Proxy

Strukturelle Näherung aus:

- Szenenlänge
- Dialoganteil
- Satzlänge
- Figuren
- Plotlinien
- Handlungsfäden

Anzeige:

- bewegter
- mittel
- ruhiger

Keine Qualitätsbewertung.

### 📈 Pacing-Verlauf

- visueller Rhythmusstreifen über alle Szenen
- direkte Navigation zur Szene
- detaillierte Szenentabelle

### 💬 Dialog-Näherung

- technische Erkennung über typische Anführungszeichen
- ausdrücklich als Näherungswert gekennzeichnet

### ◇ Struktur-Check

Hinweise unter anderem für:

- leere Szene
- POV offen
- Ort offen
- starke Längenausschläge
- sehr kurze Szenen
- lange POV-Pausen
- Fäden ohne Beats
- lange Fadenpausen
- Plotlinien ohne Szenen
- starke Kapitel-Längenausschläge

### 🔎 Filter

- Teil
- Szenenstatus
- leere Szenen ein/aus

Alle Analysen reagieren auf den aktuellen Filter.

### § Teilanalyse

Unterstützt:

- alle Teile
- einzelner Teil
- Szenen ohne Teil

### ⇩ CSV-Export

Exportiert unter anderem:

- Szene
- Kapitel
- Teil
- Status
- Wörter
- POV
- Ort
- Figuren
- Plotlinien
- Fäden
- Satzlänge
- Dialoganteil
- offene Revisionen

### ↗ Navigation

Direkt aus der Analyse erreichbar:

- Manuskriptabschnitte
- Plotting Studio
- Handlungsfäden
- Manuskript

### 🔧 Reliability

Workflow-Steuerung vereinheitlicht für:

- Plotting
- Calendar
- Codex
- Analysis

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Analysis-Routing geprüft
- Szenenlängenanalyse geprüft
- Kapitelaggregation geprüft
- POV-Verteilung geprüft
- Figurenpräsenz geprüft
- Ortsverteilung geprüft
- Plotlinienanalyse geprüft
- Fadenanalyse geprüft
- Pacing-Proxy geprüft
- Strukturhinweise geprüft
- Filter geprüft
- CSV-Export geprüft
- direkte Navigation geprüft
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen statischen Buttons/Formulare

## v0.72.0

### 🧠 Codex / Wissensnetz 3.0

- Neuer eigener Buch-Reiter „Codex“.
- Story-Bibel wird zur zentralen Wissensquelle für Manuskript, Reihe und Romanprojekt.
- Titel und Aliase können automatisch im Manuskript erkannt werden.
- Automatische Erkennung verändert den Text niemals.
- Eindeutige Erwähnungen können bewusst in echte Verknüpfungen übernommen werden.

### 🏷 Aliase / alternative Namen

Jede Story-Bibel-Akte kann beliebig viele alternative Namen besitzen.

Aliase werden verwendet in:

- Codex
- globaler Suche
- Manuskripterkennung
- Living Knowledge
- Story Graph
- Inspector

Der eigentliche Titel bleibt der kanonische Anzeigename.

### 🛑 Automatische Erkennung pro Akte abschaltbar

Neue Option:

- „Automatische Erwähnungen erkennen“

Für allgemeine Begriffe kann die Erkennung bewusst deaktiviert werden.

### 🔎 Automatische Manuskript-Erwähnungen

Erkannt werden:

- kanonischer Titel
- Aliase
- Anzahl der Erwähnungen
- verwendete Bezeichnungen
- betroffene Manuskriptabschnitte
- bereits explizit verknüpfte Abschnitte
- unverknüpfte eindeutige Erwähnungen
- mehrdeutige Bezeichnungen

### 🧠 Codex im Manuskript-Inspector

Neuer Bereich:

- 🧠 Erkannte Codex-Erwähnungen

Je Fundstelle sichtbar:

- Akte
- Typ
- Trefferzahl
- erkannter Name / Alias
- verknüpft / erkannt / mehrdeutig

Eindeutige Erwähnungen können:

- einzeln verknüpft
- gesammelt verknüpft

werden.

### ⚠ Alias-Kollisionen

Wenn derselbe Alias zu mehreren Akten gehört:

- keine automatische Entscheidung
- deutliche Mehrdeutigkeitsanzeige
- keine stillen falschen Links

### ✎ Alias-Editor

Aliase bearbeitbar:

- direkt in der Story-Bibel-Akte
- direkt aus dem Codex

Live-Prüfung auf mögliche Kollisionen.

### 📚 Wissensräume

Codex-Scope:

- Dieses Buch
- Reihe
- Romanprojekt

### 📖 Serienwissen

Der Codex zeigt:

- bandbezogenes Wissen
- Wissen für mehrere Bücher
- gemeinsam verwendete Story-Bibel-Akten

Keine Kopie pro Band nötig.

### 📊 Codex-Dashboard

Angezeigt werden:

- Anzahl Akten
- Manuskript-Erwähnungen
- explizite Links
- Alias-Mehrdeutigkeiten

Je Akte:

- Typ
- Titel
- Untertitel
- Aliase
- Wissens-Scope
- Erwähnungen
- betroffene Abschnitte
- explizite Verknüpfungen
- weitere Wissenslinks

### ＋ Erkannte Erwähnungen übernehmen

Pro Akte können eindeutige unverknüpfte Manuskript-Fundstellen gesammelt in echte Links umgewandelt werden.

Mehrdeutige Treffer bleiben unangetastet.

### 🕸 Living Knowledge 3.0

Living Knowledge zeigt jetzt auch automatische Manuskript-Referenzen.

Unterschied:

- normaler Link → gespeichert
- automatische Erwähnung → aus Manuskripttext abgeleitet

Automatische Referenzen sind sichtbar gekennzeichnet.

### 🕸 Story Graph

Eindeutige Codex-Erwähnungen erzeugen abgeleitete Verbindungen:

- Codex-Akte ↔ Manuskriptabschnitt

Diese werden visuell anders dargestellt als gespeicherte Beziehungen.

### 🔎 Globale Suche

Aliase sind vollständig durchsuchbar.

Beispiel:

- Suche: Ravi
- Ergebnis: Ravith

### 📎 Story-Bibel-Suche

Auch die Story-Bibel-Suche im Manuskript-Inspector versteht Aliase.

### 📇 Story-Bibel-Karten

Alternative Namen werden direkt auf den normalen Story-Bibel-Karten angezeigt.

### 🧬 Eine Wissensquelle

Keine getrennte Codex-Datenbank.

Dieselben Story-Bibel-Akten werden verwendet von:

- Manuskript
- Codex
- Reihe
- Suche
- Living Knowledge
- Story Graph

### 📸 Snapshots

Explizite Verknüpfungen bleiben Bestandteil des Dokumentzustands.

Automatische Referenzen werden aus dem jeweiligen Textstand neu berechnet und müssen deshalb nicht separat versioniert werden.

### 💾 Backup

Gesichert werden automatisch:

- Aliase
- Auto-Erkennungs-Einstellung
- Story-Bibel-Akten
- Buch-/Serienzuordnung
- explizite Verknüpfungen

Automatische Referenzen entstehen nach einem Import wieder direkt aus dem Manuskript.

### ⚡ Performance

- Codex-Statistiken werden pro Ansicht gesammelt.
- Story Graph scannt Manuskriptabschnitte nur einmal.
- Inspector-Erkennung läuft verzögert nach Texteingaben statt bei jedem Tastendruck.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Codex-Routing geprüft
- Alias-Datenmodell geprüft
- Alias-Editor geprüft
- Kollisionsprüfung geprüft
- globale Suche erweitert
- automatische Manuskripterkennung geprüft
- Inspector integriert
- Living Knowledge integriert
- Story Graph integriert
- Buch-/Reihen-/Projekt-Scope geprüft
- Auto-Erkennung pro Akte deaktivierbar
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare

## v0.71.0

### 📅 Calendar & Timekeeping Studio

- Neuer eigener Buch-Reiter „Kalender“.
- Eigene Story-Zeit funktioniert parallel zum normalen Kalender.
- Szenen, Timeline und Kontinuität verwenden dieselbe Zeitrechnung.

### ⚙ Eigene Kalenderdefinition

Konfigurierbar:

- Kalendername
- Ära / Jahreszusatz
- Stunden pro Tag
- Minuten pro Stunde
- eigene Wochentage
- beliebig viele Monate
- eigene Tageszahl je Monat
- Monatsreihenfolge
- beliebig viele Jahreszeiten
- Start-/Endmonat je Jahreszeit

### ✦ Schnellstart

- 12 bekannte Monate
- 12 × 30 Tage
- 13 × 28 Tage

Danach vollständig editierbar.

### 📆 Monatsansicht

- eigenes Story-Jahr
- eigener Monat
- eigene Wochenstruktur
- Ereignisse direkt auf Kalendertagen
- Jahreszeit sichtbar
- Navigation durch Monate und Jahre

### ✍ Direkt planen

- Klick auf einen Tag → neues Timeline-Ereignis
- Story-Datum automatisch vorausgefüllt

### 🎬 Szenen integriert

Neue Felder im Manuskript-Inspector:

- Story-Jahr
- Story-Monat
- Story-Tag
- Story-Stunde
- Story-Minute

### ◷ Timeline integriert

Timeline-Ereignisse können zusätzlich besitzen:

- eigenes Kalenderdatum
- eigene Story-Zeit

Die Timeline zeigt bei aktivem Kalender:

- Wochentag
- Tag
- Monatsname
- Jahr
- Ära
- Uhrzeit

### 🧭 Continuity Intelligence

Berücksichtigt jetzt auch eigene Story-Zeit bei:

- chronologisch rückwärts laufenden Szenen
- derselben Figur zur selben Zeit an verschiedenen Orten
- widersprüchlichen Kontinuitäts-Checkpoints

### 📌 Kontinuitäts-Checkpoints

Eigene Zeitrechnung auch für:

- Jahr
- Monat
- Tag
- Stunde
- Minute

### 🗂 Story-Agenda

Chronologische Übersicht über datierte:

- Szenen
- Timeline-Ereignisse
- Story-Bibel-Inhalte
- Figurenentwicklungsstationen

### 🧮 Zeitrechner

Eigene Kalenderarithmetik:

- Ausgangsdatum
- Tage addieren
- Tage abziehen
- Ziel-Datum automatisch berechnen

Dabei werden die tatsächlich definierten Monatslängen verwendet.

### 🗓 Eigene Wochen

- frei benennbare Wochentage
- Länge der Woche ergibt sich aus deren Anzahl
- automatische Wochentagsberechnung

Damit funktionieren auch z. B. 5-, 6- oder 8-Tage-Wochen.

### 🕰 Eigene Tageslänge

- Tag muss nicht 24 Stunden haben
- Stunde muss nicht 60 Minuten haben

### 📸 Snapshots & Backup

- Story-Kalenderdaten der Szenen werden automatisch mit Snapshots gesichert.
- Kalenderdefinition und Datierungen sind Bestandteil des bestehenden Komplett-Backups.
- Keine zusätzliche Datenbank.

### 🌍 Worldbuilding Studio

„Zeitrechnung & Kalender“ bleibt als Worldbuilding-/Referenzmodul bestehen.

Calendar & Timekeeping Studio übernimmt die funktionale Ebene:

- Berechnung
- Kalenderansicht
- Datierung
- Timeline
- Szenen
- Kontinuität

### 🔧 Nebenfix

- UI-Health-Audit zählt „Romanprojekt ↗“ nicht mehr fälschlich als normalen Workflow-Reiter.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Kalender-Routing geprüft
- Monatseditor geprüft
- Jahreszeiten geprüft
- eigene Wochenlänge geprüft
- eigene Tages-/Stundenlänge geprüft
- Datumsrechnung geprüft
- Szenen integriert
- Timeline integriert
- Kontinuität integriert
- Snapshot-Kompatibilität geprüft
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare

## v0.70.0

### 🌍 Worldbuilding Studio

- Story-Bibel zu einem modularen Worldbuilding Studio ausgebaut.
- Neue Story-Bibeln starten bewusst schlank.
- Kernbereiche:
  - Figuren
  - Orte
  - Welt & Lore
  - Notizen

### 🧩 Optionale Worldbuilding-Module

Einzeln aktivierbar:

- Kulturen
- Religionen & Glaube
- Staat & Herrschaft
- Organisationen
- Wirtschaft & Handel
- Sprachen & Schriften
- Spezies & Völker
- Tiere & Kreaturen
- Magie
- Technologie
- Regeln & Systeme
- Zeitrechnung & Kalender
- Gegenstände & Artefakte
- Geschichte & Ereignisse

Deaktivieren blendet Module nur aus und löscht keine Akten.

### ✦ Schnellstart-Profile

- Kompakt
- Fantasy
- Science-Fiction
- Gesellschaft & Politik
- Deep Worldbuilding

Profile bleiben anschließend vollständig individuell anpassbar.

### 🧬 Eigene Templates

Eigene spezialisierte Templates ergänzt für unter anderem:

- Kultur
- Religion
- Herrschaft
- Wirtschaft
- Sprache
- Spezies
- Magie
- Technologie
- Zeitrechnung

Bestehende Templates für Organisationen, Kreaturen, Gegenstände, Systeme und Ereignisse erweitert.

### 🧾 Eigene Weltmodule

Bestehende Funktion für eigene Story-Bibel-Reiter bleibt erhalten.

Pro eigenem Modul:

- Name
- Symbol
- Grundtyp
- eigene Template-Felder
- eigene Feldreihenfolge

### 🧠 Mindmap → Worldbuilding

Mindmap-Gedanken können jetzt direkt umgewandelt werden in:

- Kultur
- Religion
- Staat / Herrschaft
- Organisation
- Spezies
- Magie
- Technologie
- Sprache
- Wirtschaft
- Zeitrechnung
- Artefakt

Benötigte Module werden automatisch aktiviert.

### 🕸️ Vernetzung

Neue Worldbuilding-Akten funktionieren automatisch mit:

- Living Knowledge
- Story Graph
- globaler Suche
- Szenen
- Figuren
- Orten
- Timeline
- Recherche
- Mindmaps
- Reihen / Universen

### 🌱 Story Growth

Neue Worldbuilding-Akten zählen als echte Arbeit an Figuren & Welt.

Modul ein-/ausblenden erzeugt keinen künstlichen Fortschritt.

### 🔒 Daten & Kompatibilität

- bestehende Story-Bibeln behalten ihre bisherigen sichtbaren Reiter
- bestehende Akten werden nicht migriert oder dupliziert
- alte Organisationen/Gegenstände/Kreaturen/Systeme bleiben erhalten
- neue Story-Bibeln starten im Kompaktmodus
- Module ausblenden löscht keine Daten
- Komplett-Backup übernimmt Module, Templates und Inhalte automatisch

### 🔧 Nebenfix

- Plotting-Reiter in der gespeicherten Workflow-Navigation ergänzt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Worldbuilding-Typen geprüft
- Module aktiviert/deaktiviert
- Kompaktmodus geprüft
- Schnellstart-Profile geprüft
- bestehende Systemreiter werden wiederverwendet
- Templates geprüft
- Mindmap-Konvertierung geprüft
- Story Growth erweitert
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare

## v0.69.0

### 🧩 Plotting Studio

- Neuer eigener Buch-Reiter „Plotting“.
- Plotlinien, Beats, Plotstrukturen und Szenenattribute direkt mit dem bestehenden Manuskript verbunden.

### 〰 Plotlinien

Beliebig viele Plotlinien:

- Hauptplot
- Nebenplot
- Figurenbogen
- Beziehung
- Geheimnis
- Welt / Politik
- eigene Plotlinie

Mit:

- Name
- Art
- Status
- Beschreibung
- Beats

### ◆ Plotpunkte / Beats

- Titel
- Plotlinie
- Position 0–100 %
- optional Szene/Kapitel
- Funktion / Veränderung

Bei Szenenverknüpfung folgt die Beat-Position automatisch der tatsächlichen Manuskriptreihenfolge.

### 🧭 Plotlinien-Ansicht

- eigene Lane pro Plotlinie
- dramaturgische Achse 0–100 %
- Beats direkt auf der Achse
- verknüpfte Manuskriptszenen sichtbar
- direkte Bearbeitung

### ◆ Beat-Ansicht

Gesamtübersicht mit:

- Position
- Beat
- Plotlinie
- verknüpftem Manuskriptabschnitt
- Funktion

### ▦ Szenenmatrix

Standardspalten:

- Szene
- Status
- POV
- Ort
- Plotlinien
- Ziel / Konflikt
- Ergebnis

Plotlinien können direkt per Checkbox Szenen zugeordnet werden.

### ✚ Eigene Szenenattribute

Frei definierbar pro Buch.

Feldtypen:

- Text
- Auswahl
- Zahl / Skala
- Ja / Nein

Eigene Felder erscheinen direkt in der Szenenmatrix.

### ✦ Plotstruktur-Vorlagen

Enthalten:

- Drei-Akt-Struktur
- Heldenreise
- Sieben-Punkte-Struktur
- Mystery / Enthüllung
- Beziehungsbogen

Vorlagen ergänzen Beats und bleiben danach vollständig editierbar.

### 🔎 Filter

- Plotlinie
- Szenenstatus

### 📊 Plotting-Überblick

- Plotlinien
- Beats
- gekoppelte Beats
- Szenen ohne Plotlinie
- nicht aufgelöste Plotlinien

Keine Qualitätsbewertung.

### 🌱 Story Growth

Neue Wachstumsschritte:

- Plotlinie angelegt
- Plotpunkt ergänzt
- Plotstruktur übernommen

### 🕸️ Story Graph

Plotlinien sind echte Graph-Knoten.

Verbindungen:

- Plotlinie ↔ Szene
- Plotlinie ↔ Szene über Beat

### 🔎 Globale Suche

Durchsuchbar:

- Plotlinienname
- Beschreibung
- Typ
- Beat-Titel
- Beat-Beschreibung
- verknüpfte Manuskriptabschnitte

Treffer öffnen direkt das Plotting Studio.

### 🗂️ Manuscript Navigator

Plotting Studio verwendet dieselben Manuskriptszenen.

Verschieben einer Szene verändert automatisch die relative Position eines daran gekoppelten Beats.

### 📸 Snapshots

Plotlinienzuordnungen und eigene Szenenattribute liegen am Dokument und werden über den vollständigen Snapshot-Zustand mitgesichert.

### 💾 Backup

- Plotlinien und Beats am Buch
- Szenenattribute an der Szene
- automatisch im bestehenden Komplett-Backup

### 🧬 Produktprinzip

Eine Szene wird nur einmal angelegt.

Dieselbe Szene kann anschließend:

- geschrieben
- im Navigator sortiert
- dramaturgisch geplottet
- chronologisch betrachtet
- im Story Graph vernetzt
- überarbeitet
- versioniert

werden.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Plotting-Routing geprüft
- Plotlinien-CRUD geprüft
- Beat-CRUD geprüft
- Szenenverknüpfung geprüft
- Plotvorlagen geprüft
- Szenenmatrix geprüft
- freie Attribute geprüft
- Story Growth geprüft
- Story Graph geprüft
- globale Suche erweitert
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare

## v0.68.1 – Roadmap v0.67 nachgeholt

### 🗂️ Manuscript Navigator

- Roadmap-Punkt v0.67 nach v0.68 nachgeholt.
- Technische Versionsnummer v0.68.1, damit Versioning & Snapshots vollständig erhalten bleiben.

### 🌳 Neue Manuskripthierarchie

- Buch
  - Teil
    - Kapitel
      - Szene

Weiterhin möglich:

- Kapitel ohne Teil
- Szenen ohne Kapitel

### § Neuer Dokumenttyp „Teil“

- Teile können Kapitel enthalten.
- Kapitel können weiterhin frei stehen.
- Teile können eigenen Text besitzen, müssen aber nicht.

### 🧲 Drag & Drop

Unterstützt:

- Szene → Kapitel
- Kapitel → Teil
- Teile sortieren
- Kapitel innerhalb ihrer Ebene sortieren
- Szenen innerhalb eines Kapitels sortieren

Ungültige Verschachtelungen werden verhindert.

### ▾ Ein-/Ausklappen

- Teile und Kapitel einklappbar.
- Zustand pro Buch gespeichert.
- Suche/Filter zeigen Treffer trotzdem vollständig.

### 🔎 Navigator-Suche

Durchsucht unter anderem:

- Titel
- Notizen
- Kurzinhalt
- Ziel / Konflikt
- Ergebnis
- Stimmung
- POV
- Ort
- Faden
- Zeit
- Status
- Figuren

### ⌕ Filter

- Typ
- Status
- POV
- Ort
- Handlungsstrang
- nur offene Revisionen

### ☰ Strukturansicht

- echte Baumstruktur
- Hierarchieeinrückung
- Wortzahl
- Status
- Revisionen
- POV
- Ort
- Faden
- Story-Datum

### ▦ Kartenansicht

- visuelle Manuskriptkarten
- Kurzinhalt / Notiz / Vorschau
- Status
- Wortzahl
- POV
- Ort
- Faden
- Revisionen

### ⌘ Meta-Ansicht

Kompakte Übersicht über:

- Abschnitt
- Status
- POV
- Ort
- Faden
- Revisionen
- Wortzahl

### ↔ Referenz-/Split-Ansicht

- zweiten Abschnitt parallel öffnen
- per ↔ oder Ctrl/Cmd/Alt + Klick
- Haupteditor bleibt editierbar
- Referenzansicht zunächst bewusst read-only
- Grundlage für späteren echten Multi-Editor-Split

### ✂ Teilen & Zusammenführen

- Split kennt jetzt auch „Teil“.
- Elternstruktur bleibt sinnvoll erhalten.
- Zusammenführen nur noch mit gleichartigen Geschwisterabschnitten derselben Ebene.

### 🗑 Löschen

- Kinder werden beim Löschen eines Elternabschnitts nicht gelöscht.
- Sie werden freigestellt.
- tiefere Unterstruktur bleibt erhalten.

### 📦 Export

Neue Teil-Ebene berücksichtigt bei:

- Markdown
- Text
- HTML
- DOCX
- RTF
- PDF
- EPUB

### 📸 Snapshot-Kompatibilität

- v0.68 bleibt vollständig enthalten.
- Snapshots erkennen jetzt auch „Teil“.
- Strukturänderungen bleiben durch Sicherheits-Snapshots geschützt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Teil → Kapitel → Szene geprüft
- rekursive Reihenfolge geprüft
- Parent-Auswahl geprüft
- Drag-&-Drop-Regeln geprüft
- Suche und Filter geprüft
- Struktur-, Karten- und Meta-Ansicht geprüft
- Split-/Referenzansicht geprüft
- Snapshot-Kompatibilität geprüft
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare

## v0.68.0

### 📸 Versioning & Snapshots

- Einfachen Sicherheitsverlauf zum vollständigen Snapshot-Studio ausgebaut.
- Kapitel und Szenen besitzen jeweils ihren eigenen Versionsverlauf.

### 📸 Manuelle Snapshots

- jederzeit manuell anlegbar
- eigene Versionsnotiz
- manuelle Versionen unabhängig von Auto-Retention
- einzelne Snapshots mit ★ dauerhaft fixierbar

### ◷ Automatische Snapshots

Pro Buch konfigurierbar:

- an / aus
- 3 / 5 / 10 / 15 Minuten
- 20 / 50 / 100 automatische Stände behalten

Automatische Snapshots entstehen nur bei echten Änderungen.

### 🛡 Sicherheits-Snapshots

Automatische Sicherung vor unter anderem:

- Wiederherstellung
- Löschen
- Zusammenführen
- Teilen
- Strukturverschiebungen
- relevanten Metadaten-/Strukturänderungen

### 🗂 Snapshot-Filter

- Alle
- Manuell
- Sicherheit
- Automatisch

Anzeige je Snapshot:

- Typ
- Versionsnotiz
- Datum / Uhrzeit
- Wortzahl
- Kapitel oder Szene
- Differenz zum aktuellen Stand
- Fixierungsstatus

### 🔀 Versionsvergleich

- Snapshot ↔ aktueller Stand
- damalige Wortzahl
- aktuelle Wortzahl
- Wortdifferenz
- neue Absätze
- entfallene Absätze
- unveränderte Passagen
- ruhige visuelle Hervorhebung

### ↶ Historie

- direkt zur nächstälteren Version wechseln
- mehrere Fassungen nacheinander prüfen

### ♻ Wiederherstellung

- direkt aus Liste oder Vergleich
- aktueller Stand wird vorher automatisch gesichert
- versehentliche Wiederherstellung bleibt dadurch rückgängig machbar

### 🧬 Vollständiger Dokumentzustand

Neue Snapshots sichern zusätzlich zum Text unter anderem:

- Dokumenttyp
- Status
- Szenenmetadaten
- POV
- Ort
- Story-Zeit
- Handlungsstrang
- Konflikt / Ziel
- Figuren
- Ideen
- Story-Bibel
- Rechercheverknüpfungen

### 🧹 Retention

- automatische Versionen nach Buch-Einstellung begrenzt
- manuelle und Sicherheitsstände separat geschützt
- fixierte Versionen bleiben erhalten

### 💾 Pro Buch

- Snapshot-Einstellungen direkt im Buchmodell
- automatisch Bestandteil vollständiger Backups
- kein separates Versionssilo

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen Buttons/Formulare
- ältere `documentVersions` bleiben kompatibel

## v0.66.0

### ✒ Writing Studio 2.0

- Manuskript-Schreibumgebung gezielt in Richtung professioneller Editor ausgebaut, ohne Ribbon-Überladung.
- Neuer kompakter Studio-Dialog bündelt Schreibansicht, Sessionziel und Textwerkzeuge.
- Typewriter-Modus ergänzt.
- Bestehender Fokusmodus in die buchbezogenen Studio-Einstellungen integriert.
- Sessionziel in Wörtern ergänzt.
- Live-Fortschritt direkt in der Editor-Statusleiste.
- Neue Inline-Werkzeuge:
  - Kommentar
  - Fußnote
  - Textmarkierung
  - Formatierung lösen
- Kommentare und Fußnoten bleiben im Manuskript erhalten.
- Studio-Einstellungen werden pro Buch lokal gespeichert.
- Bestehende Funktionen bleiben integriert:
  - Absatzformate
  - Zeichenformatierung
  - Listen
  - Szenentrenner
  - Suchen/Ersetzen
  - Überarbeitungsnotizen
  - Dokumentnotizen
  - Autosave
  - Recovery
  - Versionen
  - Schreibsessions
- JavaScript-Syntax nach Umsetzung geprüft.

> Split View / mehrere Dokumente parallel wird gemeinsam mit dem kommenden Manuscript Navigator weiter vertieft, weil dafür Dokumentnavigation und Hierarchie zusammengehören.

## v0.65.0

### 🛠️ UX & Stability Pass

- Gezielte Stabilitäts- und Bedienbarkeitsrunde nach dem großen Vernetzungsblock.
- Fokus auf Routing, Datensicherheit, Navigation, Fehlerabfang und Performance.

### 🧭 Workflow-Navigation repariert

- Interne Liste gültiger Buchbereiche vervollständigt.
- Neuere Bereiche werden beim Wiederherstellen korrekt akzeptiert:
  - Szenenboard
  - Story Growth
  - Continuity Intelligence
  - Weltkarte
  - Character Arc Studio
  - Story Graph
- Kein stilles Zurückfallen auf ältere Standard-Reiter mehr.

### 🧹 Sauberes Panel-Routing

- Beim Öffnen/Neurendern werden jetzt konsequent alle Workflow-Panels zurückgesetzt.
- Verhindert mögliche Überlagerungen nach Routing- oder Renderproblemen.

### 🛟 Fehlerabfang pro Buchbereich

- Jeder Buchbereich läuft jetzt innerhalb einer sicheren Rendergrenze.
- Scheitert ein einzelner Bereich:
  - restliche Suite bleibt bedienbar
  - keine Storydaten werden gelöscht
  - Fehler wird protokolliert
  - ruhige Wiederherstellungsansicht erscheint
- Von dort:
  - zurück zur Übersicht
  - erneut versuchen

### 💾 Manuskript-Speicherung stabilisiert

- Keine konkurrierenden parallelen Saves mehr.
- Neue Single-Flight-Save-Queue.
- Änderungen während eines laufenden Saves werden nachgereicht.
- Bei Speicherfehler:
  - Notfallkopie bleibt erhalten
  - Speichestatus zeigt Problem
  - verständliche Meldung

### 🧯 Safety-Handler bereinigt

- doppelte `beforeunload`-Handler entfernt
- doppelte `visibilitychange`-Handler entfernt
- redundante Speicherintervalle entfernt
- klare Zuständigkeiten für:
  - Notfall-Draft
  - Hintergrund-Save
  - Safety Flush

### 🕸️ Story-Graph-Fix: Weltkarten

- Weltkarte ↔ Ort wird jetzt zuverlässig aus vorhandenen Kartenpins erzeugt.
- Kein Unterschied mehr zwischen Pin-ID in Living Knowledge und Karten-Knoten im Story Graph.

### ⚡ Story-Graph-Performance

- Beim Knotenziehen wird nicht mehr bei jeder Mausbewegung das komplette Storynetz neu berechnet.
- Nur vorhandene Kanten werden neu gezeichnet.
- Deutlich bessere Grundlage für große Storynetze.

### ⌨️ Workflow-Tastaturnavigation

- ← / → → vorheriger/nächster Reiter
- Home → erster Reiter
- End → letzter Reiter
- Enter / Leertaste → öffnen

### ↔ Lange Buchnavigation verbessert

- horizontales Scrollen bleibt erhalten
- Mausrad scrollt bei Bedarf horizontal
- aktiver Reiter wird automatisch sichtbar gemacht
- ruhigere Scrollbar
- bessere Bedienbarkeit auf kleineren Bildschirmen

### ♿ Accessibility-Basis

- Workflow-Leiste als `tablist`
- Reiter mit `role="tab"`
- `aria-selected`
- `aria-current`
- korrekter Tastaturfokus
- Panels als `tabpanel`
- Save-Status und Toast mit `aria-live="polite"`
- verbesserte sichtbare Fokusringe

### 🩺 Interner UX-Systemcheck

Beim Start geprüft:

- alle Workflow-Reiter vorhanden
- alle Workflow-Panels vorhanden
- Navigation und Panels konsistent
- keine doppelten IDs
- zentrale UI-Elemente vorhanden

Nur bei echten Auffälligkeiten erscheint eine Meldung.

### 🧹 Release-Bereinigung

- altes internes Test-Cover aus dem Release entfernt
- Paket enthält nur noch:
  - index.html
  - app.js
  - styles.css
  - README.md
  - CHANGELOG.md

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- alle Workflow-Reiter gegen Panels abgeglichen
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen statischen Buttons/Formulare
- `beforeunload` nur einmal gebunden
- `visibilitychange` nur einmal gebunden
- Story-Graph-Weltkartenkante geprüft
- Story-Graph-Pointermove optimiert
- Save-Queue geprüft
- bestehende Datenmodelle kompatibel

## v0.64.0

### 🕸️ Story Graph

- Neuer eigener Buch-Reiter „Story Graph“.
- Automatisch aus bestehenden Story-Verknüpfungen aufgebaut.
- Keine separaten Graph-Datenkopien.
- Verschieben verändert nur die Darstellung.

### ◈ Automatisch vernetzte Story-Inhalte

Darstellbar sind unter anderem:

- Szenen und Kapitel
- Figuren
- Orte
- Story-Bibel-Akten
- Ideen
- Handlungsfäden
- Timeline-Ereignisse
- Recherche
- Mindmap-Knoten
- Weltkarten

### ↔ Reale Verbindungen

Kanten entstehen aus vorhandenen Beziehungen, unter anderem:

- Living Knowledge
- Szene ↔ Story-Bibel
- Szene ↔ Idee
- Szene ↔ Recherche
- Story-Bibel ↔ Story-Bibel
- Handlungsfaden ↔ Szene
- Timeline ↔ Story-Bibel
- Mindmap-Verbindungen
- Story-Beziehungen
- Weltkarte ↔ Ort

### 🧲 Mausbedienung

- Knoten frei verschieben
- freie Fläche ziehen → Graph verschieben
- Mausrad → Zoom
- Klick → auswählen
- Doppelklick → Originalinhalt öffnen
- Rechtsklick → Aktionsmenü

### 🖱️ Rechtsklick-Menü

- Inhalt öffnen
- direktes Umfeld fokussieren
- Knoten zentrieren
- Inhaltstyp ausblenden

### ⌨️ Tastatur

- Pfeiltasten → Knoten fein bewegen
- Shift + Pfeil → größere Schritte
- Enter → Inhalt öffnen
- F → Umfeld fokussieren
- + / - → Zoom
- 0 → Netz einpassen
- Escape → Fokus/Auswahl lösen

### ◎ Fokusmodus

- ausgewählter Knoten + direkte Nachbarn
- ideal für Fragen wie:
  - Was hängt an dieser Figur?
  - Welche Szenen gehören zu diesem Faden?
  - Was ist mit diesem Ort verbunden?
- Fokus verändert keine Storydaten.

### 🔎 Suche & Typfilter

- eigene Graph-Suche
- Suche in Titel, Kontext und Inhaltstyp
- einzelne Inhaltstypen ein-/ausblendbar
- Darstellungspräferenzen pro Buch gespeichert

### ✦ Automatische Anordnung

- automatische Ausgangsanordnung
- räumliche Gruppierung nach Inhaltstyp
- „Neu anordnen“ für frisches Layout
- manuelle Positionen bleiben erhalten

### ⌂ Einpassen & Zoom

- sichtbares Netz automatisch in den Arbeitsbereich einpassen
- Zoom von 30 % bis 280 %
- freie Navigation jederzeit möglich

### 🧭 Kontext-Inspector

Zeigt beim ausgewählten Knoten:

- Inhaltstyp
- Titel
- Kontext
- Anzahl direkter Verbindungen
- verbundene Story-Inhalte
- Art der Verbindung

Mit direkter Navigation.

### 📊 Netzwerküberblick

- Story-Inhalte
- Verbindungen
- Inhaltstypen
- isolierte Inhalte

„Isoliert“ ist kein Fehlerstatus.

### 🎨 Ruhige visuelle Sprache

- keine roten Fehlerknoten
- zurückhaltende Typfarben
- aktive Verbindungen erst bei Auswahl hervorgehoben
- stärker vernetzte Knoten leicht größer

### 🧠 Story Intelligence

- direkter Zugang zum Story Graph aus Story Intelligence

### 🏠 Buch-Dashboard

- direkter Zugang zum Story Graph

### 🕸️ Living Knowledge

Living Knowledge beantwortet:

> Was hängt an diesem konkreten Inhalt?

Story Graph beantwortet:

> Wie hängt die Geschichte als Ganzes zusammen?

Beide verwenden dieselben Daten.

### 💾 Layout & Backup

- Knotenpositionen pro Buch gespeichert
- ausgeblendete Typen gespeichert
- Zoom-/Pan-Zustand gespeichert
- alles reine Darstellungsdaten
- Komplett-Backup übernimmt die Daten automatisch

### 🧬 Produktprinzip

Der Story Graph ist kein Mindmap-Ersatz.

Die Mindmap ist eine freie Denkfläche.

Der Story Graph visualisiert automatisch das bereits vorhandene Storymodell.

> Ein Inhalt wird einmal angelegt – und erscheint überall dort, wo er gebraucht wird.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft
- Story-Graph-Routing geprüft
- Maus-, Pointer-, Tastatur-, Zoom- und Kontextmenü-Handler geprüft
- automatische Graph-Erzeugung geprüft
- persistente Layoutdaten geprüft
- keine fehlenden statischen UI-Referenzen
- keine doppelten HTML-IDs
- keine doppelten Funktionen
- keine ungebundenen statischen Buttons oder Formulare
- bestehende Storydaten bleiben kompatibel

## v0.63.0

### 📖 EPUB & Publishing Export

- Das Exportzentrum kann Manuskripte jetzt direkt als EPUB-3-Datei ausgeben.
- EPUB wird als echtes E-Book-Paket mit notwendiger Paketstruktur erzeugt.

### 📚 Kapitel & Szenen

- Manuskriptkapitel werden zu EPUB-Inhaltsabschnitten.
- Szenen werden ihrem Kapitel zugeordnet.
- Szenen ohne Kapitel bleiben exportierbar.
- Szenentitel können optional ausgegeben werden.
- Ohne Szenentitel werden Szenen durch ruhige Szenentrenner getrennt.

### 🧭 Inhaltsverzeichnis

- Automatisches EPUB-3-Navigationsdokument.
- Kapitelreihenfolge entspricht dem Manuskript.
- Inhaltsverzeichnis verlinkt direkt auf die jeweiligen Kapitel.

### 🏷️ Publishing-Metadaten

Neue Buchfelder:

- Autor:in
- Sprache
- eigener BCP-47-Sprachcode
- Rechte / Copyright
- optionale EPUB-Kennung

Zusätzlich verwendet werden:

- Titel
- Untertitel
- Reihe
- Bandnummer
- ISBN
- Verlag / Imprint
- Veröffentlichungsdatum

### 🖼️ Cover

- Vorhandenes Buchcover kann eingebettet werden.
- Cover wird im EPUB-Manifest als echtes Coverbild markiert.
- Automatische Coverseite.
- EPUB bleibt auch ohne Cover exportierbar.

### 🖼️ Bilder im Manuskript

- Eingebettete Data-URL-Bilder werden als echte EPUB-Bilddateien übernommen.
- Unterstützt:
  - PNG
  - JPEG / JPG
  - WEBP
  - GIF

### ✨ Titelseite

Optional automatisch mit:

- Titel
- Untertitel
- Autor:in
- Reihe
- Bandnummer

### 🎨 EPUB-Stylesheet

Enthalten sind Reader-freundliche Regeln für:

- Kapitel
- Szenen
- Absätze
- Szenentrenner
- Zitate
- Listen
- Bilder
- Titelseite
- Coverseite

### 🧹 XHTML-Aufbereitung

- Manuskript-HTML wird für EPUB bereinigt.
- Scripts, Formulare, Buttons, Frames usw. werden entfernt.
- Absätze, Hervorhebungen, Zitate, Listen, Links und Bilder bleiben erhalten.
- Reiner Text wird automatisch in Absatzstruktur überführt.

### ✓ EPUB-Prüfung

Vor dem Export geprüft:

- Titel
- Manuskriptinhalt
- Sprachcode
- Autor:in
- Cover
- Kapitelstruktur

Hinweise und echte Exportblocker werden getrennt dargestellt.

### 🧱 Paket-Strukturprüfung

Beim Erzeugen zusätzlich geprüft:

- `mimetype`
- `META-INF/container.xml`
- OPF-Paketdatei
- Navigationsdatei
- Stylesheet
- Inhaltsverzeichnis
- doppelte Dateipfade

Die interne Prüfung ersetzt noch kein externes offizielles EPUBCheck.

### 📦 Exportoptionen

- Cover einbetten
- Szenentitel ausgeben
- Titelseite erzeugen

Die Optionen verändern nur den Export, niemals das Manuskript.

### 📎 Buchakte erweitert

- Autor:in
- Sprache
- EPUB-Kennung

werden auch in der Buchakte sichtbar.

### 🔒 Portabilität

- Neue Metadaten liegen direkt im Buch-Datenmodell.
- Komplett-Backups sichern sie automatisch.
- Keine neue Publishing-Datenablage.
- Alte Bücher bleiben kompatibel.

### 🧬 Produktprinzip

EPUB verwendet dieselben:

- Manuskriptkapitel
- Szenen
- Cover
- Buchdaten
- Publikationsdaten

wie der Rest der Suite.

Kein Export-Silo.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- EPUB-Workflow geprüft.
- Publishing-Metadaten geprüft.
- Cover- und Bildverarbeitung geprüft.
- interne EPUB-Strukturprüfung ergänzt.
- keine fehlenden statischen UI-Referenzen.
- keine doppelten HTML-IDs.
- keine doppelten Funktionen.
- keine ungebundenen statischen Buttons oder Formulare.
- bestehende TXT-, MD-, HTML-, DOCX-, RTF-, PDF-, Projekt- und Backup-Exporte bleiben erhalten.

## v0.62.0

### 📚 Living Bookshelf 2.0

- Das bestehende realistische Bücherregal wurde zum visuellen Projektcockpit ausgebaut.
- Bücher stehen weiterhin direkt nebeneinander.
- Zusätzliche Informationen erscheinen erst bei Hover oder Tastaturfokus.
- Klick öffnet weiterhin das Buch selbst.

### ✨ Erweiterte Hover-Karte

Phaseabhängige Informationen.

Beim Schreiben:

- Wortzahl
- Kapitel
- Szenen
- offene Handlungsfäden

In der Planung:

- Story-Growth-Schritte der letzten 7 Tage
- Szenen im Entstehen
- verknüpfte Ideen
- offene Planungsfragen

In der Überarbeitung:

- Wortzahl
- offene Überarbeitungsnotizen
- offene Handlungsfäden
- Kontinuitäts-Prüfstellen

### 🌱 Story Growth am Regal

- Planungsbücher zeigen ihre aktuelle Story-Growth-Aktivität.
- Aktive Planungstage und Wachstumsschritte stammen direkt aus Story Growth.
- Keine künstliche Planungs-Prozentzahl.

### 🧵 Offene Story-Arbeit

Berücksichtigt werden:

- offene Handlungsfäden
- offene Überarbeitungsnotizen
- offene Planungsfragen
- Kontinuitäts-Prüfstellen

Ein dezenter grüner Punkt am Buchrücken kann vorhandenen Arbeitskontext anzeigen.

### ◷ Letzte Aktivität

Anzeige beispielsweise:

- zuletzt geschrieben: heute
- zuletzt geschrieben: gestern
- zuletzt geplant: vor 3 Tagen
- letzte Änderung: 04.08.2026

### 📊 Living-Bookshelf-Puls

Kompakte Übersicht für die aktuell sichtbaren Bücher:

- aktive Bücher
- Manuskriptwörter
- Story Growth · 7 Tage
- offene Fäden
- offene Revisionen

### 📕 Visuelle Projektphase bleibt erhalten

- Entwurf → Notizbuch
- Planung → Planner / Ringbuch
- Schreiben → Buch
- Überarbeitung → Revisionsbuch
- fertig / veröffentlicht → fertiges Buch

Auch die dynamische Buchdicke bleibt erhalten.

### 🧘 Ruhiges Regal

- Keine dauerhaften Kennzahlen auf den Buchrücken.
- Keine Dashboard-Karten zwischen den Büchern.
- Bücher bleiben direkt aneinander.
- Details erst bei bewusster Interaktion.

### ⌨️ Tastatur

- Buchrücken bleiben fokussierbare Buttons.
- Fokus zeigt dieselbe Infokarte wie Hover.
- Enter / Leertaste öffnet das Buch.

### 🧬 Produktprinzip

Living Bookshelf erzeugt keine neuen Daten.

Es verwendet:

- Manuskript
- Story Growth
- Szenen
- Fäden
- Revision
- Continuity Intelligence
- Schreibsessions
- Buchmetadaten

Das Regal wird zur visuellen Eingangstür in das bestehende Storymodell, nicht zu einem neuen Informationssilo.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Living-Bookshelf-Puls geprüft.
- Phaseabhängige Hover-Kennzahlen geprüft.
- Letzte-Aktivität-Ermittlung geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionen.
- Keine ungebundenen statischen Buttons oder Formulare.
- Bestehende Buchrücken-, Cover-, Status- und Buchdickenlogik bleibt erhalten.

## v0.61.0

### 👤 Character Arc Studio

- Neuer eigener Buch-Reiter „Figurenbogen“.
- Figurenentwicklung basiert direkt auf den bestehenden Story-Bibel-Figuren.
- Kein paralleles Figurenmodell.

### 🧭 Entwicklungsstationen

Eine Station kann enthalten:

- Buch
- Szene / Kapitel
- Story-Datum
- Story-Zeit
- Station / Wendepunkt
- Glaube / Überzeugung
- Wunsch
- Wissen
- emotionaler Zustand
- Beziehungen / Nähe
- Konflikt
- Entscheidung
- Veränderung
- freie Notiz

### ➝ Visuelle Entwicklungsachse

- Stationen werden entlang des Figurenbogens visualisiert.
- Szenenverknüpfte Stationen orientieren sich an der Manuskriptreihenfolge.
- Frei eingeordnete Stationen bleiben möglich.
- Stationen sind direkt anklick- und bearbeitbar.

### 🔁 Anfang ↔ aktueller Stand

Automatischer Vergleich zwischen erster und letzter Entwicklungsstation:

- Glaube
- Wunsch
- Wissen
- Emotion
- Beziehungen

### 🎬 Story-Kontext

- Manuskriptabschnitte mit Figurenbezug werden direkt angezeigt.
- Story-Datum, Uhrzeit, Ort und Handlungsstrang werden soweit vorhanden eingeblendet.
- Direkter Sprung zur Szene.
- Szene kann direkt mit einer Entwicklungsstation verbunden werden.

### ◷ Timeline-Integration

- Datierte Entwicklungsstationen erscheinen in der Story-Timeline.
- Mit datierten Szenen verknüpfte Stationen können ebenfalls dort erscheinen.
- Neuer Timeline-Typ „Figurenentwicklung“.
- Keine doppelten Timeline-Daten.
- Klick führt zurück zur jeweiligen Arc-Station.

### 🧠 Entwicklungsdimensionen

Pro Station werden acht Dimensionen betrachtet:

- Glaube
- Wunsch
- Wissen
- Emotion
- Beziehungen
- Konflikt
- Entscheidung
- Veränderung

Die Anzeige x / 8 beschreibt nur vorhandene Tiefe und ist keine Fertig-Prozentzahl.

### 🔎 Figuren-Navigation

- Eigene Figuren-Seitenleiste.
- Figurensuche.
- Anzahl von Entwicklungsstationen.
- Manuskriptvorkommen.
- letzter dokumentierter emotionaler Zustand.

### 🕸️ Living Knowledge

- Szenen, die über Entwicklungsstationen verbunden sind, werden als Wissenszusammenhang erkannt.
- Szenen erkennen umgekehrt die zugehörige Figurenakte.

### 🔎 Globale Suche

Durchsuchbar sind jetzt zusätzlich:

- Stationsname
- Glaube
- Wunsch
- Wissen
- Emotion
- Beziehungen
- Konflikt
- Entscheidung
- Veränderung
- Arc-Notizen

### 🌱 Story Growth

- Neue Entwicklungsstationen zählen als Planungsarbeit.
- Kategorie: Figuren & Welt.
- Bearbeiten einer bestehenden Station erzeugt keinen künstlichen neuen Wachstumsschritt.

### 🔧 Timeline-Nebenfix

- Timeline-Navigation akzeptiert jetzt sowohl die bestehenden `:`-IDs als auch ältere Kurzformen.
- Dadurch werden Timeline-Sprünge robuster.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Character-Arc-Routing geprüft.
- Dialogfelder und Formbindungen geprüft.
- Timeline-Integration geprüft.
- Story-Growth- und Living-Knowledge-Integration geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionen.
- Keine ungebundenen statischen Buttons oder Formulare.

## v0.60.1

### 🗺️ Living World Maps – Ausbau

- Neue ein-/ausblendbare Kartenebenen:
  - Orte
  - Szenen
  - Timeline-Ereignisse
  - Figuren
- Szenen werden an ihren verknüpften Story-Bibel-Orten dargestellt.
- Timeline-Ereignisse können direkt auf der Karte erscheinen.
- Figurenpositionen werden aus vorhandenen Szenendaten abgeleitet.
- Story-Datumsfilter ergänzt.
- Figurenfilter ergänzt.
- Für ausgewählte Figuren können aus datierten Szenen automatisch Reiserouten dargestellt werden.
- Karteninhalte sind direkt navigierbar:
  - Szene → Manuskript
  - Ereignis → Timeline
  - Figur → Story-Bibel
- Orts-Pins können unabhängig von den Story-Ebenen ein-/ausgeblendet werden.
- Keine doppelten Storydaten: Karte verwendet weiterhin dieselben Orte, Szenen, Figuren und Ereignisse wie die übrige Suite.
- Maus-, Tastatur-, Drag-&-Drop-, Zoom- und Pin-Bedienung aus v0.60 bleibt erhalten.

### Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionsdefinitionen.
- Keine Datenmigration notwendig.

## v0.60.0

### 🗺️ World & Map Studio

- Neuer eigener Buch-Reiter „Weltkarte“.
- Eigene Kartenbilder als PNG, JPG oder WEBP hochladen.
- Mehrere Karten pro Buch möglich.
- Karten können umbenannt und gelöscht werden.
- Karten und Pins werden vollständig lokal gespeichert.

### ⌖ Echte Story-Bibel-Orte als Kartenpins

- Jeder Pin verweist auf eine echte Ortsakte.
- Keine losen Textmarker.
- Ein Ort kann pro Karte nur einmal verankert werden.
- Erneutes Platzieren verschiebt den vorhandenen Pin.
- Pin löschen löscht niemals die Ortsakte.

### 🖱️ Mausbedienung

- Ort anklicken → Platzierungsmodus.
- Danach auf die gewünschte Kartenposition klicken.
- Alternativ Drag & Drop aus der Ortsliste.
- Pins frei mit der Maus verschieben.
- Doppelklick auf Pin → Ortsakte öffnen.
- Freie Kartenfläche ziehen → Karte verschieben.
- Mausrad → Zoom.

### ⌨️ Tastaturbedienung

Bei ausgewähltem Pin:

- Pfeiltasten → fein bewegen
- Shift + Pfeil → größere Schritte
- Enter → Ortsakte öffnen
- Entf / Backspace → Pin entfernen

Allgemein:

- + → hineinzoomen
- - → herauszoomen
- 0 → Ansicht zurücksetzen
- Enter → ausgewählten neuen Ort in Kartenmitte setzen
- Escape → Platzierung abbrechen

### 🧲 Drag & Drop

- Ortskarten sind echte Drag-&-Drop-Quellen.
- Ablegen auf der Karte speichert die konkrete Position.
- Relative Prozentpositionen halten Pins auch bei anderen Bildschirmgrößen stabil.

### 🔍 Zoom & Navigation

- Zoom von 35 % bis 400 %.
- Mausrad, Buttons und Tastatur unterstützt.
- Kartenansicht kann frei verschoben werden.
- „Ansicht“ setzt Zoom und Verschiebung zurück.

### 🗂️ Mehrere Karten

Zum Beispiel:

- Weltkarte
- Kontinent
- Stadt
- Palast
- Gebäude
- Dungeon

### 📎 Story-Bibel-Integration

- Ortsliste verwendet die bestehenden Ortsakten des Buches.
- Hierarchische Orte zeigen ihren übergeordneten Ort.
- „＋ Ort“ führt direkt zur Ortsanlage.
- Falls nötig, kann automatisch eine Story-Bibel für das Buch entstehen.

### 🕸️ Living Knowledge

- Kartenpins zählen als echte Wissensverknüpfung.
- Bei einem Ort erscheint die zugehörige Weltkarte unter „Was hängt hier dran?“.
- Von dort kann direkt zur Karte gesprungen werden.
- Lösen der Kartenverknüpfung entfernt nur den Pin.

### 🔎 Globale Suche

- Weltkarten sind global auffindbar.
- Suchindex berücksichtigt Kartenname, Buch und darauf platzierte Orte.
- Treffer öffnen direkt das betreffende Buch und die Karte.

### 🌱 Story Growth

Neue Wachstumsereignisse:

- Weltkarte angelegt
- Ort auf Karte verankert

Reines Verschieben eines Pins erzeugt bewusst keinen künstlichen Wachstumsschritt.

### 📦 Export & Backup

- Kartenbilder sind Bestandteil des Buch-Datenmodells.
- Komplett-Backups enthalten Karten und Pins automatisch.
- Backup-Inhaltsprüfung zählt Kartenbilder mit.
- Projektdossiers führen Karten und verknüpfte Orte auf.

### 📐 Kartenpositionen

- Pinpositionen werden relativ in Prozent gespeichert.
- Stabil bei:
  - anderen Monitorgrößen
  - responsive Layout
  - Zoom
  - erneutem Öffnen

### 🧬 Produktprinzip

Die Karte ist kein Informationssilo.

Ein Kartenpunkt bleibt derselbe Ort, der auch in Story-Bibel, Szenen, Timeline, Living Knowledge und späterer Continuity-/Knowledge-Logik verwendet wird.

### 🔒 Kompatibilität

- Bestehende Bücher erhalten automatisch eine leere Kartenliste.
- Alte Projekte bleiben nutzbar.
- Keine destruktive Datenmigration.
- Löschen von Karte oder Pin löscht keine Ortsakten.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Workflow-/Routing geprüft.
- Datei-Upload geprüft.
- Maus-, Tastatur- und Drag-&-Drop-Handler statisch geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionen.
- Keine ungebundenen statischen Buttons oder Formulare.

## v0.59.0

### 🧭 Continuity Intelligence 2.0

- Neuer eigener Buch-Reiter „Kontinuität“.
- Regelbasierte Prüfung ausschließlich auf Basis explizit vorhandener Storydaten.
- Ruhige Prüfhinweise statt Fehlerampeln.

### ◷ Chronologieprüfung erweitert

- Datierte Manuskriptszenen werden gegen ihre Manuskriptreihenfolge geprüft.
- Zeitliche Rücksprünge werden als mögliche Prüfstelle sichtbar.
- Gemeinsame Timeline-Aufbereitung für Manuskript, Ereignisse und Story-Bibel ergänzt.

### ⌖ Figuren an mehreren Orten

- Datum + Uhrzeit + Ort + Figurenbezug können gegeneinander geprüft werden.
- Dieselbe Figur zur gleichen Uhrzeit an unterschiedlichen Orten erzeugt einen Hinweis.

### 📌 Kontinuitäts-Checkpoints

Checkpoints können jetzt enthalten:

- Buch
- Kategorie
- Bezeichnung
- Wert
- Story-Datum
- Story-Uhrzeit
- verknüpfte Szene
- Zusatznotiz

Kategorien:

- Aufenthaltsort
- Wissensstand
- Zustand
- Besitz
- Beziehung
- Alter
- freie Notiz

### ◇ Widersprüchliche Zustände

- Unterschiedliche Angaben derselben Kategorie zum selben Zeitpunkt werden erkannt.
- Gilt bereits für Ort, Wissen, Zustand, Besitz, Beziehungen, Alter und freie Kategorien.

### ⌖ Szene ↔ Aufenthaltsort

- Aufenthalts-Checkpoints können Szenen zugeordnet werden.
- Abweichende Ortsangaben werden als Prüfhinweis angezeigt.

### ◷ Checkpoint ↔ Szenendatum

- Eigenes Checkpoint-Datum und Datum einer verknüpften Szene werden verglichen.
- Abweichungen werden sichtbar gemacht.

### 🎂 Altersprüfung

- Charakterakten mit echtem Geburtsdatum können gegen datierte Alters-Checkpoints geprüft werden.
- Abweichungen werden nur angezeigt, wenn ausreichend eindeutige Daten vorhanden sind.

### 📊 Kontinuitäts-Center

Zeigt:

- mögliche Prüfstellen
- datierte Szenen
- Szenen mit Ort
- Szenen mit Figuren
- Checkpoints
- Story-Akten mit Verlauf

Filter:

- Chronologie
- Aufenthaltsort
- Wissensstand
- Zustand
- Besitz
- Beziehung
- Alter

### ↗ Direkte Navigation

- Prüfhinweise führen zur betroffenen Szene oder Story-Bibel-Akte.
- Checkpoints öffnen direkt die zugehörige Akte.
- Schnellzugriff auf die Timeline.

### 🧠 Story Intelligence integriert

- Kontinuitäts-Prüfstellen werden in Story Intelligence berücksichtigt.
- Direkter Wechsel zum ausführlichen Kontinuitäts-Center.
- Direkter Zugang auch aus dem Buch-Dashboard.

### 🌱 Story Growth integriert

- Neue Kontinuitäts-Checkpoints zählen als Planungsarbeit.
- Sie werden automatisch als Strukturarbeit in Story Growth erfasst.

### 🧬 Vorbereitung späterer Systeme

Die Version legt bereits die Grundlage für:

- Character Knowledge
- State Tracking
- Besitzverläufe
- Beziehungsverläufe
- tiefere Kontinuitätsprüfung

Die großen Knowledge-/State-Systeme bleiben aber bewusst ihren späteren Roadmap-Versionen vorbehalten.

### 🔒 Kompatibilität

- Alte Kontinuitätsnotizen bleiben gültig.
- Sie werden automatisch als „freie Notiz“ behandelt.
- Neue Felder sind optional.
- Keine destruktive Migration.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionsdefinitionen.
- Keine ungebundenen statischen Buttons oder Formulare.
- Fehlende gemeinsame Timeline-Aufbereitung aus älteren Versionen ergänzt.

## v0.58.0

### 🌱 Story Growth

- Jedes Buch besitzt jetzt einen eigenen Workflow-Reiter „Story Growth“.
- Planung wird damit als eigenständige kreative Arbeitsphase sichtbar gemacht.
- Keine künstliche Anzeige wie „Roman zu 63 % geplant“.
- Stattdessen wird sichtbar, wie die Geschichte tatsächlich wächst und konkreter wird.

### ✦ Automatisches Planungs-Tracking

Story Growth protokolliert ab dieser Version automatisch unter anderem:

- mit dem Buch verknüpfte Ideen
- auf die Planungswand übernommene Ideen
- neu angelegte Szenen
- konkretisierte Szenen
- neue Planungsbausteine
- neue Figurenakten
- neue Ortsakten
- neues Weltwissen / Story-Bibel-Wissen
- neue Handlungsfäden
- Timeline-Ergänzungen
- neue Mindmap-Gedanken
- neue Wissensverknüpfungen
- geklärte Planungsfragen

### 🎬 Szenen-Konkretisierung

- Planungsszenen erhalten intern einen einfachen Konkretisierungswert.
- Berücksichtigt werden:
  - Kurzinhalt
  - POV
  - Ort
  - Zeit
  - Figuren
  - Ziel / Konflikt
  - Ergebnis
  - Stimmung
  - Handlungsstrang
  - Ideen-/Story-Bibel-Verknüpfungen
- Neue ausgefüllte Dimensionen werden als Wachstumsschritt erfasst.
- Änderungen aus Planung und Manuskript-Inspector werden berücksichtigt.
- Der Wert ist ausdrücklich keine Fertig-Prozentzahl.

### 📊 Eigener Story-Growth-Bereich

- Wachstumsschritte der letzten 7 Tage
- aktive Planungstage der letzten 30 Tage
- Szenen im Entstehen
- Wissensverknüpfungen
- Kategorien:
  - Ideen
  - Szenen
  - Figuren & Welt
  - Struktur
  - Vernetzung
  - geklärte Fragen
- 14-Tage-Verlauf
- aktueller Projektstand
- letzte Wachstumsschritte

### 🌿 Planungsrhythmus statt Streak-Druck

- Aktive Planungstage werden sichtbar.
- Keine „Serie verloren“-Mechanik.
- Pausentage werden nicht negativ bewertet.

### 🧠 Aktueller Story-Stand

Zusätzlich sichtbar:

- verknüpfte Ideen
- Szenen
- Figurenakten
- Ortsakten
- Handlungsfäden
- durchschnittliche Szenen-Kontur
- Wissensverknüpfungen

Bestehende Inhalte werden angezeigt, ohne rückwirkend Aktivitätsdaten zu erfinden.

### ❓ Offene Planungsfragen

- Planungsfragen pro Buch festhalten.
- Direkt in Story Growth anlegen.
- Als geklärt markieren oder wieder öffnen.
- Geklärte Fragen zählen als Story-Growth-Schritt.
- Fragen können unabhängig von anderen Storydaten entfernt werden.

### 📚 Dashboard-Integration

- Direkter Story-Growth-Zugang im Buch-Dashboard.
- Nach Aktivität erscheint ein kleiner Wochen-Puls.
- Er zeigt Zahl und Kurzfassung der aktuellen Wachstumsschritte.

### 🔄 Integration

Story Growth hängt direkt an bestehenden Funktionen:

- Ideenarchiv
- Planungswand
- Szenenplanung
- Manuskript
- Story-Bibel
- Handlungsfäden
- Timeline
- Mindmaps
- Living Knowledge

Kein paralleles Planungssystem.

### 🔒 Datenmodell & Portabilität

- Story-Growth-Ereignisse werden direkt beim Buch gespeichert.
- Planungsfragen ebenfalls.
- Bestehende Bücher bleiben kompatibel.
- Backups übernehmen die neuen Daten automatisch.
- Keine destruktive Migration.
- Pro Buch werden maximal die letzten 2.500 Wachstumsschritte gespeichert.

### 🧬 Produktprinzip

Story Growth bewertet nicht, ob eine Geschichte gut oder fertig geplant ist.

Es beantwortet nur:

> Was ist an meiner Geschichte in letzter Zeit gewachsen?

Damit entstehen langfristig:

- 🌱 Story Growth – Planen
- ✒ Writing Progress – Schreiben
- ✓ Revision Growth – Überarbeiten

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Story-Growth-Routing geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionen.
- Keine ungebundenen statischen Buttons oder Formulare.
- Bestehende Bücher und Planungsdaten bleiben kompatibel.

## v0.57.0

### 🕸️ Living Knowledge / Wissensnetz 2.0

- Story-Bibel-Akten besitzen jetzt direkt in ihrer Bearbeitungsansicht den Bereich „Living Knowledge – Was hängt hier dran?“.
- Die Ansicht wird automatisch aus den bereits vorhandenen Verknüpfungen der Suite aufgebaut.
- Inhalte werden nicht kopiert und es entsteht kein zweites Wissenssystem.

### 🔎 Automatische Kontextansicht

Für eine Story-Bibel-Akte werden – soweit vorhanden – automatisch zusammengeführt:

- Szenen und Kapitel
- Ideen
- Handlungsfäden
- Timeline-Ereignisse
- Recherche
- andere Story-Bibel-Akten
- Mindmap-Knoten
- Beziehungen

Die Bereiche werden gruppiert dargestellt und zeigen jeweils ihre Trefferzahl.

### ↗ Direkte Navigation

- Jeder Eintrag im Wissensnetz ist direkt anklickbar.
- Ein Klick führt zum tatsächlichen Originalinhalt in der Suite.
- Damit wird die Story-Bibel stärker zum Index der Geschichte statt zu einer isolierten Ablage.

### ＋ Verknüpfungen direkt anlegen

Über „＋ Verknüpfen“ können Story-Bibel-Akten jetzt direkt verbunden werden mit:

- Szene / Kapitel
- Idee
- Handlungsfaden
- Timeline-Ereignis
- Recherche
- anderer Story-Bibel-Akte
- freiem Mindmap-Knoten

### ↔ Bidirektionales Wissensnetz

- Neue Wissensverknüpfungen werden an der jeweils fachlich passenden Stelle des vorhandenen Datenmodells gespeichert.
- Bestehende Dokument-, Faden-, Timeline- und Mindmap-Verknüpfungen werden weiterverwendet.
- Ideen und Recherche können ergänzend direkt mit Story-Bibel-Akten verbunden werden.
- Story-Bibel-Akten können sich gegenseitig direkt verknüpfen.

### × Verknüpfungen lösen

- Verknüpfungen können direkt aus der Living-Knowledge-Ansicht wieder gelöst werden.
- Das Lösen löscht niemals den verknüpften Inhalt selbst.
- Gegenseitige Aktenverknüpfungen werden auf beiden Seiten sauber entfernt.
- Mindmap-Knoten bleiben erhalten und werden wieder zu freien Knoten.

### ◇ Isolierte Inhalte sichtbar

- Story-Bibel-Karten zeigen die Anzahl ihrer Wissensverknüpfungen.
- Eine unverknüpfte Akte wird dezent als „isoliert“ gekennzeichnet.
- Das ist ausdrücklich kein Fehlerstatus.

### 🧭 Globale „Was hängt hier dran?“-Ansicht verbessert

- Verknüpfungen werden nach Inhaltstyp gruppiert.
- Pro Gruppe wird die Trefferzahl angezeigt.
- Isolierte Inhalte erhalten einen verständlichen leeren Zustand.

### 🧹 Sauberes Löschverhalten

Beim Löschen einer Story-Bibel-Akte werden ihre Referenzen gezielt bereinigt, ohne die verknüpften Inhalte zu löschen.

### 🧬 Produktprinzip

- Living Knowledge verwendet vorhandene Storydaten als gemeinsame Quelle.
- Kein neues Informationssilo.
- Das Wissensnetz ist eine Sicht auf reale Verknüpfungen – keine parallele Kopie derselben Information.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionsdefinitionen.
- Keine ungebundenen statischen Buttons oder Formulare.
- Bestehende Daten bleiben kompatibel.

## v0.56.0

### 🎬 Szenenworkflow 2.0

- Neuer eigener Reiter „Szenenboard“ innerhalb eines Buches.
- Vier Workflow-Stufen:
  - Idee & geplant
  - geschrieben
  - überarbeiten
  - fertig
- Manuskriptszenen können per Drag & Drop zwischen den Statusstufen verschoben werden.
- Noch nicht übertragene Planungsszenen erscheinen bereits im Szenenboard.

### 🧩 Erweiterte Szenenplanung

Planungsszenen können jetzt speichern:

- Szenentitel
- Kurzinhalt
- POV
- Ort
- Zeit / Einordnung
- beteiligte Figuren
- Ziel / Konflikt
- Ergebnis / Veränderung
- Handlungsstrang
- Stimmung
- verknüpfte Ideen
- verknüpfte Story-Bibel-Einträge

### 🔄 Planung und Manuskript verbunden

- Szenenmetadaten werden vollständig ins Manuskript übernommen.
- Die Manuskriptszene merkt sich ihren ursprünglichen Planungsbaustein.
- Änderungen an der Planung werden auf die bestehende Manuskriptszene übertragen.
- Änderungen im Manuskript-Inspektor werden zurück in die Planung gespiegelt.
- Eine bereits übertragene Szene wird bei erneuter Übergabe nicht dupliziert.
- Damit entwickelt sich derselbe Szenenbaustein durch den gesamten Workflow.

### ↗ Szenenboard → Manuskript

- Manuskriptszenen können direkt aus dem Szenenboard geöffnet werden.
- „Details“ führt direkt zum Szenen-Inspektor.
- Planungsszenen lassen sich direkt aus dem Board bearbeiten.

### 🏷️ Einheitlicher Status

- geplant → Idee & geplant
- Rohfassung → geschrieben
- überarbeiten → überarbeiten
- fertig → fertig

Es wurde bewusst kein paralleles zweites Statussystem eingeführt.

### 🔗 Bestehendes Datenmodell

- Figuren
- Orte
- Ideen
- Story-Bibel
- Handlungsstränge

verwenden die bereits vorhandenen Verknüpfungen der Suite.

### 🔒 Kompatibilität

- Bestehende Manuskriptszenen bleiben nutzbar.
- Alte Planungsszenen bleiben gültig.
- Neue Felder sind ergänzend.
- Keine destruktive Datenmigration erforderlich.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Szenenboard- und Planungsreferenzen geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionsdefinitionen.
- Keine ungebundenen statischen Buttons oder Formulare.

## v0.55.0

### 📦 Exportzentrum

- Zentrales Exportzentrum für Manuskripte, Projekte und Komplett-Backups.
- Erreichbar aus Buchwerkzeugen und Datensicherheit.
- Klare Trennung zwischen weitergebbaren Dokumenten und internen Projektdaten.

### ✒ Manuskript-Export

- TXT
- Markdown
- HTML
- DOCX
- RTF
- PDF
- Exportiert nur Buchtitel, Untertitel, Kapitel-/Szenentitel und Manuskripttext.

### 📄 DOCX

- Echtes DOCX direkt im Browser erzeugt.
- Keine externe Bibliothek oder Cloud erforderlich.
- In Word und LibreOffice weiterbearbeitbar.

### PDF

- Mehrseitige lokale PDF-Ausgabe.
- Kapitel und Szenen typografisch unterschieden.
- Als saubere Lese-/Kontrollfassung gedacht.

### ▤ Projektdossier

- Markdown
- HTML
- JSON
- Enthält u. a. Bücher, Story-Bibel, Figuren, Orte, Weltwissen, Handlungsfäden, Timeline, Recherche und relevante Ideen.

### 🔗 Maschinenlesbares Projektpaket

- JSON enthält zusätzlich Manuskript, Beziehungen, Mindmaps, Knoten, Verbindungen und weitere relationale Projektdaten.
- Für vollständige Wiederherstellung bleibt das Komplett-Backup das maßgebliche Format.

### 🔒 Geprüftes Komplett-Backup

- Bestehende vollständige Sicherung direkt ins Exportzentrum integriert.
- Vorhandene Validierung und Rollback-Mechanismen bleiben erhalten.

### ✓ Backup-Inhaltsbericht

- Zeigt vor dem Backup:
  - Ideen
  - Kategorien
  - Projekte
  - Bücher
  - Manuskriptabschnitte
  - Anhänge
  - Story-Bibel-Akten
  - Mindmaps
  - Bilder
  - geprüfte Datenbereiche
- Separat als JSON exportierbar.

### 🖼️ Bilder & Portabilität

- Buchcover, Story-Bibel-Bilder und Inspirationsbilder werden im Backup-Bericht berücksichtigt.
- Eingebettete Bilder bleiben im vollständigen Backup erhalten.
- Manuskript-Ausgaben bleiben bewusst frei von internen Story-Bibel-Bildern.

### 🔒 Lokale Verarbeitung

- Alle Exportformate werden vollständig lokal erzeugt.
- Keine Datenübertragung an Server oder Cloud-Dienste.
- Keine zusätzlichen externen Exportbibliotheken.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionen.
- Keine ungebundenen statischen Buttons oder Formulare.
- Backup-/Importformat bleibt kompatibel.

## v0.54.0

### 🧪 Vorab-Audit: Navigation, Links & neue Workflows

- JavaScript-Syntax vollständig geprüft.
- Statische UI-IDs gegen alle direkten JavaScript-Referenzen geprüft.
- Keine fehlenden statischen UI-Referenzen gefunden.
- Keine doppelten HTML-IDs gefunden.
- Keine doppelten Funktionsdefinitionen gefunden.
- Statische Buttons und Formulare auf Handler bzw. Submit-/Delegationslogik geprüft.
- Hauptnavigation und Buch-Workflow-Routing geprüft.
- Die neuen Bereiche aus v0.49 bis v0.53 wurden dabei mit berücksichtigt.

### 🔧 Zwei echte Verknüpfungsfehler behoben

- „＋ Projekt anlegen“ im Bücherregal öffnet wieder zuverlässig den Projekt-Editor.
- Mit Romanideen verknüpfte Mindmap-Knoten lassen sich jetzt direkt öffnen.

### 🧠 Mindmap 2.0 – semantische Knoten

- Knotenarten:
  - freier Gedanke
  - Idee
  - Figur
  - Ort
  - Szene
  - Kapitel
  - Handlungsfaden
  - Recherche
  - Buch
- Knotenart und grafische Form bleiben unabhängig voneinander.
- Verknüpfte Inhalte können ihre semantische Art automatisch anzeigen.

### ↗ Echte Suite-Inhalte direkt öffnen

- Verknüpfte Knoten zeigen einen anklickbaren Verknüpfungs-Chip.
- Rechtsklick → „Verknüpften Inhalt öffnen“.
- Direkter Sprung zum tatsächlichen Inhalt der Suite.

### ✦ Gedanken in echte Inhalte umwandeln

- Unterstützte Ziele:
  - Idee
  - Charakterakte
  - Ortsakte
  - Szene
  - Kapitel
  - Handlungsfaden
  - Recherche
  - Buch
- Der Mindmap-Knoten bleibt bestehen.
- Der neu erzeugte Inhalt wird automatisch zurück mit dem Knoten verknüpft.

### 📎 Mindmap → Story-Bibel

- Figur erzeugt eine echte Charakterakte.
- Ort erzeugt eine echte Ortsakte.
- Reiter-Templates aus v0.53 werden übernommen.
- Falls nötig, wird automatisch eine Story-Bibel für das Zielbuch angelegt.

### ✒ Mindmap → Manuskript

- Knoten können zu Kapitel oder Szene werden.
- Zielbuch und Romanprojekt werden gewählt bzw. vorausgefüllt.
- Szenen können optional direkt einem Kapitel zugeordnet werden.
- Neue Manuskriptabschnitte starten als „geplant“.

### 🧵 Mindmap → Handlungsfaden

- Knoten können direkt zu echten Handlungsfäden werden.
- Titel und Beschreibung werden übernommen.
- Der Faden startet als „angelegt“.

### ◉ Mindmap → Recherche

- Knoten können direkt als Rechercheeintrag gespeichert werden.
- Projektbezug wird übernommen.

### 📚 Mindmap → Buch

- Ein Gedanke kann als neues Entwurfsbuch innerhalb eines Romanprojekts angelegt werden.
- Knotentitel → Buchtitel.
- Knotennotiz → erste Buchnotiz.

### 💡 Mindmap → Idee

- Die bisherige Einzelfunktion wurde in den allgemeinen Umwandlungsworkflow integriert.
- Projekt-/Buchbezug wird nach Möglichkeit direkt übernommen.

### 🖱️ Rechtsklick-Menü erweitert

- Bearbeiten
- verbundenen Gedanken erstellen
- Verbindung herstellen
- Inhalt verknüpfen
- verknüpften Inhalt öffnen
- in Suite-Inhalt umwandeln
- löschen

### 🔒 Datenmodell & Sicherheit

- Bestehende Mindmaps bleiben kompatibel.
- Farben, Formen, Positionen und Verbindungen bleiben erhalten.
- Umwandlungen löschen den ursprünglichen Knoten nicht.
- Keine destruktive Datenmigration erforderlich.

### ✓ Abschlussprüfung

- JavaScript-Syntax geprüft.
- Keine fehlenden statischen UI-Referenzen.
- Keine ungebundenen statischen Buttons oder Formulare.
- Keine doppelten HTML-IDs.
- Keine doppelten Funktionsdefinitionen.

## v0.53.0

### 📎 Story-Bibel mit eigenen Reitern

- „Welt & Orte“ wurde direkt in den bestehenden Anhang / die Story-Bibel integriert.
- Jeder Anhang besitzt jetzt eine Reiterleiste.
- Standardmäßig stehen bereit:
  - Charaktere
  - Orte
  - Welt & Lore
  - Organisationen
  - Gegenstände
  - Kreaturen
  - Regeln & Systeme
  - Ereignisse
  - Notizen
- Eigene Reiter können zusätzlich frei angelegt werden.
- Reiter können einen eigenen Namen und ein eigenes Symbol erhalten.
- Lange Reiterleisten scrollen horizontal.

### 🧩 Eigene Templates pro Reiter

- Jeder Story-Bibel-Reiter besitzt ein eigenes Template.
- Pro Reiter kann ein Grundtyp gewählt werden.
- Template-Felder können individuell:
  - angelegt
  - benannt
  - typisiert
  - entfernt
  - sortiert
  werden.
- Unterstützt werden Text, Langtext, Zahl, Datum und Link.
- Neue Einträge übernehmen automatisch das Template des aktuellen Reiters.
- Einzelne Steckbriefe bleiben danach weiterhin individuell anpassbar.
- Bestehende Einträge werden nicht überschrieben.

### 👤 Charaktere

- Charaktere liegen standardmäßig in einem eigenen Reiter.
- Die Figurenentwicklungsstationen aus v0.52 bleiben vollständig erhalten.
- Charakter-Templates können pro Anhang angepasst werden.

### ⌖ Welt & Orte

- Orte sind als eigener Story-Bibel-Bereich organisiert.
- Neue Standardfelder:
  - Art des Ortes
  - Region / Land
  - Klima / Jahreszeiten
  - Atmosphäre
  - Sinneseindrücke
  - Bewohner
  - Kultur / Alltag
  - Gefahren / Besonderheiten
  - Bedeutung für die Handlung
- „Welt & Lore“ besitzt ebenfalls ein erweitertes Weltbau-Template.

### 🗺️ Hierarchische Orte

- Ortsakten können einen übergeordneten Ort besitzen.
- Möglich sind z. B.:
  - Welt
  - Land
  - Region
  - Stadt
  - Gebäude
  - Raum
- Im Orte-Reiter erscheint automatisch eine kompakte Ortshierarchie.
- Klick auf einen Ort öffnet direkt seine Akte.
- Nachfahren werden als möglicher übergeordneter Ort ausgeschlossen, um Kreisverknüpfungen zu vermeiden.

### 📁 Ordner innerhalb der Reiter

- Ordner und Unterordner bleiben erhalten.
- Neu angelegte Ordner gehören zum aktuell geöffneten Reiter.
- Ältere vorhandene Ordner bleiben kompatibel.

### 🔄 Bestehende Story-Bibeln

- Vorhandene Einträge werden anhand ihres bisherigen Typs automatisch passenden Standardreitern zugeordnet.
- Keine manuelle Migration nötig.
- Eigene Reiter können entfernt werden, ohne darin liegende Einträge zu löschen.
- Standardreiter können angepasst, aber nicht versehentlich gelöscht werden.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine fehlenden UI-Referenzen gefunden.
- Keine doppelten IDs oder Funktionen gefunden.
- Keine destruktive Datenmigration erforderlich.
- Bestehende Anhänge, Bilder, Ordner, Figurenentwicklungen und Verknüpfungen bleiben erhalten.

## v0.52.0

### ✦ Story Intelligence

- Jedes Buch besitzt jetzt einen eigenen Workflow-Tab „Story Intelligence“.
- Der Bereich wertet ausschließlich bereits vorhandene Daten der Suite aus:
  - Handlungsfäden
  - Manuskriptabschnitte
  - Timeline
  - Story-Bibel
  - Kontinuitätsnotizen
  - Figurenverknüpfungen
  - Überarbeitungsnotizen
- Die Hinweise sind bewusst als Unterstützung formuliert und nicht als automatische Fehlerbewertung.

### 🧵 Story-Radar für offene Fäden

- Handlungsfäden werden jetzt zusätzlich als Story-Radar ausgewertet.
- Die Suite erkennt unter anderem:
  - offene Fäden ohne Manuskriptstation
  - Fäden, die über viele Abschnitte nicht mehr aufgegriffen wurden
  - als aufgelöst markierte Fäden mit nur einer einzigen Station
- Der Fadenstatus wurde erweitert:
  - angelegt
  - entwickelt
  - zugespitzt
  - aufgelöst
  - fallengelassen
- Bestehende Statuswerte bleiben kompatibel.
- Story-Intelligence-Karten führen direkt zum jeweiligen Handlungsfaden.

### ♙ Figurenentwicklung

- Charakterakten in der Story-Bibel können jetzt Entwicklungsstationen enthalten.
- Pro Station können festgehalten werden:
  - Buch
  - konkrete Szene / konkretes Kapitel
  - Station / Wendepunkt
  - was die Figur glaubt
  - was sie will
  - was sie weiß
  - emotionaler Zustand
  - freie Notiz
- Entwicklungsstationen werden als horizontale Figurenbogen-Leiste dargestellt.
- Sie können direkt aus Story Intelligence angelegt, bearbeitet und gelöscht werden.
- Die Suite weist dezent darauf hin, wenn eine häufig auftretende Figur noch keine Entwicklungsstation besitzt.

### ◷ Chronologie-Hinweise

- Die bereits vorhandene Timeline-Prüfung ist jetzt zusätzlich in Story Intelligence eingebunden.
- Mögliche gleichzeitige Aufenthalte derselben Figur an unterschiedlichen Orten werden als Hinweis angezeigt.
- Zusätzlich werden auffällige Sprünge zwischen Story-Chronologie und Manuskriptreihenfolge sichtbar gemacht.
- Die Timeline selbst bleibt weiterhin die Quelle für chronologische Daten.

### 📌 Kontinuitäts- & Strukturhinweise

- Story Intelligence weist unter anderem auf folgende Situationen hin:
  - häufig auftretende Figur ohne Entwicklungsstation
  - Entwicklungsstationen ohne Szenenverknüpfung
  - vorhandene bandspezifische Kontinuitätsnotizen
  - längere Szene ohne Kurzinhalt
  - als fertig markierte Szene mit noch offenen Überarbeitungsnotizen
- Hinweise öffnen – sofern möglich – direkt die betreffende Szene, Figur oder den betreffenden Faden.

### 📊 Story-Übersicht

- Der neue Bereich zeigt kompakt:
  - offene Fäden
  - Figurenakten
  - Chronologie-Hinweise
  - offene Überarbeitungsaufgaben
- Das Buch-Dashboard zeigt jetzt zusätzlich die Zahl der aktuellen Story-Hinweise und verlinkt direkt zu Story Intelligence.

### 🔒 Datenmodell & Sicherheit

- Figurenentwicklungsstationen werden direkt in der jeweiligen Story-Bibel-Charakterakte gespeichert.
- Bestehende Charakterakten erhalten automatisch eine leere Entwicklungsstruktur.
- Bestehende Handlungsfäden, Timeline-Daten und Manuskripte werden nicht verändert.
- Keine destruktive Datenmigration erforderlich.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Neue Dialog-, Panel- und Workflow-Referenzen geprüft.
- Keine doppelten IDs oder Funktionen gefunden.
- Bestehende Bücher und Story-Daten bleiben kompatibel.

## v0.51.0

### ✦ Überarbeitungsmodus

- Bücher besitzen jetzt einen eigenen Workflow-Tab „Überarbeitung“.
- Bücher mit Status „Überarbeitung“ öffnen standardmäßig direkt diesen Bereich.
- Die Überarbeitung ist als Kanban-Board aufgebaut:
  - Offen
  - In Arbeit
  - Erledigt
- Jede Aufgabe bleibt mit dem konkreten Kapitel bzw. der konkreten Szene verbunden.

### 📝 Überarbeitungsnotizen direkt am Manuskript

- Im Editor gibt es jetzt „✦ Überarbeiten“.
- Eine Notiz kann allgemein für den aktuellen Abschnitt angelegt werden.
- Markierter Manuskripttext kann direkt mit einer Überarbeitungsnotiz verknüpft werden.
- Rechtsklick im Manuskript öffnet dafür ein eigenes Aktionsmenü.
- Der ausgewählte Text wird als Zitat an der Aufgabe gespeichert, ohne den Manuskripttext zu verändern.
- „Im Text“ führt zurück in den passenden Manuskriptabschnitt und sucht die gespeicherte Passage.

### 🗂️ Kategorien

- Plot
- Figuren
- Logik
- Stil
- Recherche
- Kontinuität
- Kürzen
- Ausbauen

### 🧭 Überarbeitung im Inspector

- Eigener Inspector-Tab „Überarbeitung“.
- Alle Aufgaben des aktuellen Kapitels bzw. der aktuellen Szene sichtbar.
- Neue Aufgaben direkt dort anlegen.
- Vorhandene Aufgaben öffnen und bearbeiten.

### 📚 Kapitel-/Szenenstatus

- Die Manuskript-Navigation zeigt die Zahl offener Überarbeitungsaufgaben.
- Erledigte Aufgaben zählen dort nicht mehr mit.
- Der normale Manuskriptstatus bleibt davon unabhängig.

### 🔄 Aufgabenworkflow

- Aufgaben zwischen Offen, In Arbeit und Erledigt wechseln.
- Aufgaben bearbeiten oder löschen.
- Löschen einer Aufgabe verändert niemals den Manuskripttext.

### 🔒 Daten & Sicherheit

- Überarbeitungsnotizen werden direkt beim jeweiligen Manuskriptabschnitt gespeichert.
- Bestehende Kapitel und Szenen erhalten automatisch eine leere Überarbeitungsliste.
- Keine destruktive Datenmigration.
- Autosave, Sicherheitsversionen und Manuskripttext bleiben unangetastet.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Alle neuen Dialog- und UI-Referenzen geprüft.
- Keine doppelten IDs oder Funktionen gefunden.
- Bestehende Bücher und Manuskripte bleiben kompatibel.

## v0.50.0

### ⌘ Globale Suche

- Strg+K bzw. Cmd+K öffnet die suiteweite Suche.
- Durchsucht werden unter anderem:
  - Ideen
  - Romanprojekte und Bücher
  - Figuren und Orte
  - Kapitel und Szenen inklusive Manuskripttext
  - Story-Bibel und Anhänge
  - Handlungsfäden und Timeline
  - Mindmaps und Mindmap-Knoten
  - Recherche
  - Inspiration
  - Namenslisten
- Manuskriptabschnitte werden zusätzlich über Notizen, Ziel/Konflikt, Stimmung, Ergebnis und verknüpfte Inhalte gefunden.
- Treffer bleiben nach Inhaltstyp gruppiert.

### ⌨️ Tastaturbedienung der Suche

- Pfeil hoch/runter bewegt die Auswahl.
- Enter öffnet den markierten Treffer.
- Escape schließt die Suchpalette.
- Mausbedienung bleibt erhalten.

### 🔗 „Wo hängt das dran?“

- Suchtreffer zeigen an, wenn direkte Verknüpfungen vorhanden sind.
- Ein Klick auf die Verknüpfungszahl öffnet eine eigene Übersicht.
- Dort wird sichtbar, wo ein Inhalt verwendet bzw. womit er verbunden ist.
- Unterstützt werden unter anderem:
  - Idee → Romanprojekt, Buch, Planung, Manuskript, Mindmap
  - Buch → Projekt, Ideen, Manuskript, Story-Bibel, Handlungsfäden, Timeline, Mindmaps
  - Story-Bibel-Eintrag → Manuskript, Beziehungen, Timeline, Handlungsfäden, Mindmaps
  - Kapitel/Szene → Buch, Projekt, Ideen, Story-Bibel, Recherche, Inspiration, Handlungsfäden
  - Figur/Ort → Projekt und verwendende Manuskriptabschnitte
  - Recherche/Inspiration → Manuskriptabschnitte und Mindmaps
  - Mindmap-Knoten → Mindmap, verknüpfter Suite-Inhalt und benachbarte Knoten
- Alle Einträge der Verknüpfungsübersicht sind direkt anklickbar.

### 🧭 Verknüpfungen statt Datensilos

- Vorhandene Verknüpfungen werden ausgewertet, nicht dupliziert.
- Das Anzeigen der Verknüpfungen verändert keine Daten.
- Suchindex und Verknüpfungsanzeige werden nach Änderungen neu aufgebaut.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Bestehende Bücher, Manuskripte, Mindmaps, Anhänge und Ideen bleiben kompatibel.

## v0.49.0

### 🎬 Szenenplanung

- Szenentrenner auf der Buch-Planungswand sind jetzt echte Szenenplaner statt bloßer Trenner.
- Für geplante Szenen können zusätzlich festgehalten werden:
  - Kurzinhalt
  - Ziel / Konflikt
  - Ergebnis / Veränderung
  - Stimmung
- Die wichtigsten Angaben erscheinen kompakt direkt auf der Szenenkarte der Planungswand.
- Die bestehende Drag-&-Drop-Reihenfolge der Planung bleibt erhalten.

### ✒ Planung → Manuskript

- Beim Übergeben einer geplanten Szene ins Manuskript werden ihre Szenendaten mitgenommen.
- Kurzinhalt, Ziel/Konflikt, Ergebnis und Stimmung stehen anschließend im Inspector des Manuskripts zur Verfügung.
- Die geplante Szene wird damit zum selben inhaltlichen Objekt im Schreibworkflow, statt beim Übergang ihre Planungsinformationen zu verlieren.
- Bereits übergebene Planungsblöcke werden weiterhin erkannt und nicht unbemerkt dupliziert.

### 📌 Schnellablage im Schreibflow

- Im Manuskripteditor gibt es jetzt „＋ Gedanke“.
- Ein Gedanke kann festgehalten werden, ohne den aktuellen Schreibabschnitt verlassen zu müssen.
- Zur Auswahl stehen:
  - Idee
  - Überarbeiten
  - Recherche
  - Kontinuität
  - Szenennotiz
- Neue Ideen werden automatisch mit dem aktuellen Romanprojekt, Buch und Manuskriptabschnitt verknüpft.
- Recherchegedanken werden als Rechercheeintrag angelegt und mit dem Abschnitt verbunden.
- Szenennotizen landen direkt beim aktuellen Abschnitt.
- Nach dem Speichern springt der Fokus zurück in den Manuskripttext.

### 🧭 Szenen-Inspector

- Der Inspector zeigt zusätzlich „Ergebnis / Veränderung“ und „Stimmung“.
- Diese Angaben werden wie die übrigen Szenendaten automatisch gespeichert.
- Aus der Planung übernommene Werte können beim Schreiben jederzeit weiterentwickelt werden.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Bestehende Planungswände, Manuskripte und Bücher bleiben kompatibel.
- Keine destruktive Datenmigration notwendig.

## v0.48.2

### 📚 Hierarchische Roman-/Buchauswahl

- Der Statistikfilter zeigt Romanprojekte und ihre Bücher jetzt hierarchisch statt als flache Liste.
- Die Reihe steht als übergeordnete Gruppe.
- Darunter erscheinen die einzelnen Bücher eingerückt.
- Zusätzlich kann weiterhin die gesamte Reihe ausgewählt werden.
- Einzelne Bücher können jetzt direkt als Statistikfilter gewählt werden.
- Lange Auswahllisten bleiben als Dropdown kompakt und werden scrollbar dargestellt.

### 🏷️ Anzeigename einzelner Bücher

- In den Buchdaten gibt es jetzt den Bereich „Anzeige in Buchlisten“.
- Für jedes Buch kann festgelegt werden, wie es unter seiner Reihe in kompakten Auswahllisten erscheint:
  - automatisch – Untertitel wird bevorzugt
  - Untertitel
  - Bandnummer + Untertitel
  - Titel
  - eigener Anzeigename
- Bei „Eigener Anzeigename“ erscheint ein zusätzliches Eingabefeld.
- Bestehende Bücher verwenden automatisch die neue intelligente Anzeige.
- Wenn mehrere Bücher denselben Haupttitel tragen, kann der Reihentitel einmal übergeordnet und darunter nur der unterscheidende Untertitel angezeigt werden.

### 📊 Statistikfilter

- Die Statistik kann nun auf alle Romanprojekte, eine gesamte Reihe/ein Romanprojekt oder ein einzelnes Buch eingegrenzt werden.
- Buchbezogene Wortzahlen und Schreibsessions berücksichtigen weiterhin die bestehende Einstellung zum Schreibtracking.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Bestehende Bücher und Statistikdaten bleiben kompatibel.

## v0.48.1

### 🔷 Verschiedene Formen für Mindmap-Knoten

- Jeder Mindmap-Knoten kann jetzt eine eigene Form bekommen.
- Zur Auswahl stehen:
  - Karte
  - abgerundete Karte
  - Kapsel
  - Kreis
  - Raute
  - Wolke
  - Notizzettel
- Die Form wird pro Knoten gespeichert und bleibt beim Verschieben, Verknüpfen und erneuten Öffnen erhalten.
- Bestehende Knoten verwenden automatisch weiterhin die klassische Kartenform.

### 👁️ Formvorschau

- Im Knoten-Dialog gibt es eine direkte Vorschau der gewählten Form.
- Ein Wechsel im Auswahlfeld aktualisiert die Vorschau sofort.
- Farbe und Form bleiben voneinander unabhängig kombinierbar.

### 🧠 Bedeutung durch Form

- Formen können frei verwendet werden, z. B.:
  - Kreis für Figuren
  - Raute für Entscheidungen
  - Wolke für lose Ideen
  - Kapsel für kurze Stichworte
  - Notizzettel für offene Gedanken
- Die Suite erzwingt bewusst keine feste Bedeutung.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Bestehende Mindmaps bleiben kompatibel.

## v0.48.0

### 🧠 Mindmaps – intuitive Mausbedienung

- Rechtsklick auf einen Mindmap-Knoten öffnet jetzt ein eigenes Aktionsmenü.
- Verfügbar sind:
  - Bearbeiten
  - verbundenen Gedanken erstellen
  - Verbindung herstellen
  - Suite-Inhalt verknüpfen
  - Löschen
- „Verbundenen Gedanken erstellen“ legt einen neuen Knoten in der Nähe des Ausgangsknotens an und verbindet beide automatisch.
- „Verbindung herstellen“ startet einen klaren Verbindungsmodus; anschließend genügt ein Klick auf den zweiten Knoten.
- Doppelklick auf eine freie Stelle der Mindmap legt dort direkt einen neuen Gedanken an.
- Ein Klick auf einen Knoten markiert ihn sichtbar.
- Ein Klick auf die freie Fläche hebt die Auswahl wieder auf.

### ⌨️ Mindmaps – Tastatursteuerung

- Enter öffnet den markierten Knoten zum Bearbeiten.
- Entf/Backspace löscht den markierten Knoten nach Sicherheitsabfrage.
- Pfeiltasten verschieben den markierten Knoten fein.
- Shift + Pfeiltasten verschiebt ihn in größeren Schritten.
- Strg/Cmd + Enter legt einen neuen, automatisch verbundenen Gedanken an.
- Escape beendet den Verbindungsmodus und hebt die Auswahl auf.
- Die Mindmap-Leiste enthält einen dezenten Hinweis auf Maus- und Tastaturbedienung.

### 🔗 Verbindungen

- Der bestehende Verbindungsdialog bleibt erhalten und kann jetzt zusätzlich über das Rechtsklick-Menü gestartet werden.
- Beim Löschen eines Knotens werden seine Verbindungen sauber mit entfernt.
- Neu verbundene Gedanken verwenden weiterhin das bestehende Mindmap-Datenmodell.

### 🎛️ Scrollbars im Suite-Design

- Die grauen Standard-Browser-Scrollbars wurden durch schmalere, abgerundete Scrollbars in warmen Braun-/Papierfarben ersetzt.
- Vertikale und horizontale Scrollbars verwenden dieselbe Designsprache.
- Hoverzustände sind dunkler, ohne wie Fehler- oder Warnfarben zu wirken.
- Die Mindmap-Scrollfläche verwendet eine passend abgestimmte Variante.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Bestehende Mindmaps, Knoten und Verbindungen bleiben kompatibel.

## v0.47.9

### 📚 Lebendiges Bücherregal

- Alle Bücher einer Regalebene besitzen jetzt dieselbe optische Höhe.
- Entwicklungsstände verändern Buchtyp und Dicke statt die Buchhöhe.
- Entwurf/Konzept erscheint als weiches Notizbuch.
- Planung erscheint als Ringbuch/Projektplaner mit sichtbarer Spiralbindung.
- Schreiben erscheint als klassisches gebundenes Buch.
- Überarbeitung erhält ein dezentes Leseband.
- Fertige/veröffentlichte Bücher wirken massiver und abgeschlossen.

### 📏 Buchdicke nach Inhalt

- Entwurfsbücher orientieren sich an gesammelten Ideen und Planung.
- Planungsbücher orientieren sich an Ideen, Planungsblöcken und Kapitelmarkern.
- Schreib- und Überarbeitungsbücher orientieren sich an der aktuellen Manuskript-Wortzahl.
- Fertige/veröffentlichte Bücher verwenden bevorzugt die hinterlegte Seitenzahl.
- Fehlt die Seitenzahl, wird sie aus endgültiger Wortzahl bzw. Manuskriptumfang angenähert.
- Mindest- und Maximalbreite verhindern unsichtbar dünne oder absurd breite Bücher.

### 🖱️ Hoverinformationen

- Die Hoverkarte zeigt die für die Dicke relevante Größe: Ideen/Planungsblöcke, Wörter oder Seiten.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Bestehende Coverfarben, Reihendaten und Buchverknüpfungen bleiben erhalten.

## v0.47.8

### 📚 Ein durchgehendes Bücherregal

- Die Bücherübersicht ist jetzt ein echtes, zusammenhängendes Regal statt einzelner Projektblöcke.
- Projekttitel und Status-Zwischenüberschriften unterbrechen das Regal nicht mehr.
- Alle sichtbaren Bücher stehen direkt nebeneinander.
- Weitere Bücher laufen auf der nächsten Regalebene weiter.
- Projekt, Status, Wortzahl und weitere Informationen bleiben über die Hover-/Fokuskarte erreichbar.
- Projekt- und Statusfilter funktionieren weiterhin, ohne die Regaloptik aufzuteilen.

### ✨ Reihen beim Buchanlegen schneller auswählen

- Das Feld „Reihe“ schlägt beim Tippen bereits vorhandene Reihennamen vor.
- Existiert die gewünschte Reihe noch nicht, kann der neue Name einfach eingetippt werden.
- Beim Speichern wird der neue Reihenname direkt verwendet.
- Vorhandene Reihen werden alphabetisch in der Vorschlagsliste geführt.

### 🗂️ Ideenarchiv – neue Unterkategorien alphabetisch

- Neu über das Kontextmenü angelegte Unterkategorien werden sofort alphabetisch zwischen ihren Geschwistern einsortiert.
- Sie erscheinen nicht mehr pauschal ganz oben bzw. an einer unpassenden Position.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Bestehende Bücher, Projekte, Reihen und Kategorien bleiben erhalten.

## v0.47.7

### 📊 Schreibstatistik pro Buch steuerbar

- Jedes Buch besitzt jetzt die Einstellung „Schreibstatistik aufzeichnen“.
- In „Entwurf“ und „Planung“ ist Tracking grundsätzlich deaktiviert.
- Neue Bücher im Status „Schreiben“ oder „Überarbeitung“ aktivieren das Tracking standardmäßig.
- Fertige bzw. veröffentlichte Bücher sind standardmäßig vom Tracking ausgeschlossen.
- Der Schalter kann bei Schreib- und Überarbeitungsbüchern jederzeit ein- oder ausgeschaltet werden.
- Ausgeschlossene Bücher erzeugen keine neuen Schreibsessions.
- Bereits vorhandene Sessions ausgeschlossener Bücher werden in Statistiken nicht mehr mitgerechnet, aber nicht gelöscht.
- Gesamtwortzahl, Projektaufschlüsselung, Schreibzeit, Serien, Tages-/Wochenwerte und Diagramme respektieren die Einstellung.
- Der Statistikbereich zeigt an, wie viele Bücher vom Tracking ausgeschlossen sind.

### ✍️ Manuelle Wortzahl-Einträge

- Manuelle Wortzahl-Einträge können jetzt einem konkreten Buch zugeordnet werden.
- Bücher mit deaktiviertem Tracking werden entsprechend gekennzeichnet.
- Für ausgeschlossene Bücher können keine versehentlichen Statistik-Einträge erzeugt werden.
- Projektweite Einträge ohne konkretes Buch bleiben möglich.

### 📁 Ordner & Unterordner im Anhang

- Story-Bibeln unterstützen frei benannte Ordner und verschachtelte Unterordner.
- Neue Einträge können direkt einem Ordner zugeordnet werden.
- Ordner können umbenannt und gelöscht werden.
- Beim Löschen werden Einträge und Unterordner sicher eine Ebene nach oben verschoben.
- Inhalte werden nicht mit dem Ordner gelöscht.
- Geteilte Anhänge teilen automatisch auch ihre komplette Ordnerstruktur.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Bestehende Bücher erhalten anhand ihres Status eine sinnvolle Tracking-Voreinstellung.
- Keine Datenmigration erforderlich.
- Vorhandene Schreibsessions und Anhang-Einträge werden nicht gelöscht.

## v0.47.6

### 🐛 Planungswand – eigentliche Render-Ursache behoben

- Fehler behoben, durch den die Planungswand genau dann abstürzte, wenn ein Ideenzettel auf ihr lag.
- Eine lokale Darstellungsvariable überschrieb versehentlich den globalen App-Zustand beim Rendern eines Ideenblocks.
- Ideenzettel auf der Planungswand werden jetzt wieder korrekt aus dem Ideenarchiv geladen.
- Ältere Zahl-/Text-IDs werden auch innerhalb der Planungsblöcke robust erkannt.
- Die Sicherheitsmeldung aus v0.47.5 bleibt als Schutz bestehen, sollte künftig aber nicht mehr durch diesen Fehler ausgelöst werden.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Vorhandene Planungsblöcke und Ideen bleiben erhalten.

## v0.47.5

### 🧩 Planungswand – Ideenübernahme repariert

- „Auf Planungswand“ legt die gewählte Idee jetzt zuverlässig in die Buchplanung.
- Nach der Übernahme wird die Planungsansicht explizit geöffnet und gerendert.
- Bestehende Verknüpfungen mit älteren Zahl-/Text-IDs werden robust erkannt.
- Bereits geplante Ideen werden unabhängig vom internen ID-Typ erkannt.
- Die Planungswand erzwingt bei aktivem Planungstab eine sichtbare Arbeitsfläche.
- Falls das Rendern scheitert, erscheint statt einer leeren braunen Fläche eine Fehlermeldung mit „Planung erneut laden“.
- Gespeicherte Planungsdaten werden bei einem Darstellungsfehler nicht verändert.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Bestehende Buch-, Projekt- und Ideenverknüpfungen bleiben erhalten.

## v0.47.4

### 🔗 Verknüpfungen auf Ideenzetteln sichtbar korrigiert

- Roman-/Buchkennzeichnungen auf den Zetteln deutlich sichtbarer gestaltet.
- Anzeige jetzt z. B. als „↗ Roman: Diener des Himmels“.
- Projektverknüpfungen erscheinen entsprechend als „↗ Projekt: …“.
- Kategorie und Romanverknüpfung werden sauber getrennt dargestellt.
- Lange Bezeichnungen werden platzsparend gekürzt.
- Hover zeigt die vollständige Zuordnung.
- Klick öffnet direkt Buch bzw. Projekt.

### 🧰 Kompatibilitätsfix für bestehende Verknüpfungen

- Bestehende/importierte Verknüpfungs-IDs werden unabhängig davon erkannt, ob sie als Zahl oder Text gespeichert wurden.
- Dadurch werden auch ältere vorhandene Roman-/Buchverknüpfungen zuverlässig in der Ideenübersicht erkannt.
- Der Filter „Verknüpft mit“ verwendet dieselbe robuste Erkennung.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration erforderlich.
- Bestehende Daten werden nicht verändert.

## v0.47.3

### 🔗 Roman-/Buchverknüpfungen direkt auf Ideenzetteln sichtbar

- Verknüpfte Bücher bzw. Romanprojekte werden jetzt direkt in der Ideenübersicht angezeigt.
- Ein Buch erscheint als dezentes Badge mit Buchtitel.
- Bei mehreren Büchern wird das erste Buch plus Anzahl weiterer Verknüpfungen angezeigt.
- Nur auf Projektebene verknüpfte Ideen erhalten ein Projekt-Badge.
- Hover zeigt die vollständigen Verknüpfungen.
- Klick auf ein Badge öffnet direkt Buch oder Romanprojekt.
- Verknüpfungsfarben verwenden Petrol/Salbei bzw. Blau statt Warnfarben.

### 🔎 Nach Romanprojekt oder Buch filtern

- Im Filterdialog gibt es jetzt „Verknüpft mit“.
- Romanprojekte und einzelne Bücher können gewählt werden.
- Der Filter lässt sich mit Kategorie, Suche, Favoriten und weiteren Filtern kombinieren.
- Projektfilter berücksichtigen auch Ideen, die über ein Buch des Projekts verknüpft sind.
- Die Trefferanzeige nennt den aktiven Projekt-/Buchfilter.

### 🖱️ Rechtsklick auf Ideenzettel korrigiert

- Konflikt zwischen zwei gleichzeitig gesetzten Rechtsklick-Handlern behoben.
- Rechtsklick öffnet jetzt zuverlässig das Aktionsmenü.
- Mehrfachauswahl über Shift/Strg und Auswahlkreis bleibt erhalten.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenmigration notwendig.
- Bestehende Projekt- und Buchverknüpfungen werden direkt weiterverwendet.

## v0.47.2

### 🔗 Ideen direkt mit Romanen & Büchern verknüpfen

- Im Detailfenster jedes Ideenzettels gibt es jetzt den Bereich „Mit Roman / Buch verknüpfen“.
- Bücher werden nach Romanprojekt gruppiert angezeigt.
- Eine Idee kann mit mehreren Büchern verknüpft werden.
- Durch die Buchzuordnung wird sie automatisch auch im zugehörigen Romanprojekt gesammelt.
- Bereits verknüpfte Bücher erscheinen direkt am Ideenzettel und können dort wieder gelöst werden.
- Der ursprüngliche Zettel bleibt immer im Ideenarchiv; es wird keine Kopie erzeugt und keine Kategorie verändert.

### 🖱️ Rechtsklick-Menü für Ideenzettel

- Rechtsklick auf einen Zettel öffnet jetzt ein Kontextmenü.
- Verfügbar sind:
  - Öffnen / bearbeiten
  - Mit Roman / Buch verknüpfen
  - Favorisieren bzw. Favorit entfernen
  - Verschieben
  - Duplizieren
  - Papierkorb
- Die Aktionen verwenden die bereits vorhandenen Speicher- und Sicherheitsfunktionen.

### ☰ Burgermenü funktioniert jetzt

- Das bisher funktionslose Burger-Symbol oben links öffnet jetzt ein echtes Hauptmenü.
- Von dort erreichbar sind Dashboard, Ideenarchiv, Romanideen, Bücher & Projekte, Namenslisten, Recherche, Inspiration, Mindmaps und Einstellungen.
- Klick außerhalb oder auf das X schließt das Menü wieder.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Bestehende Buch-/Projekt-Verknüpfungsstruktur wird wiederverwendet.
- Keine neue Datenbankstruktur notwendig.
- Bestehende Daten bleiben kompatibel.

## v0.47.1

### ✒ Manuskript-UI neu ausbalanciert

- Die stabile Scrolllogik aus v0.47.0 bleibt erhalten.
- Der Schreibbereich nutzt die verfügbare Fensterbreite besser.
- Unnötiges horizontales Seiten-Scrolling wurde entfernt.
- Kapitelspalte und Inspector wurden schmaler.
- Der eigentliche Schreibbereich wurde deutlich verbreitert.

### 🧭 Kopfbereich beruhigt

- Buchauswahl, Wortfortschritt und Aktionen wurden kompakter angeordnet.
- Aktionsbuttons dürfen sauber umbrechen.
- Wortzahl und Fortschritt sind visuell weniger dominant.

### 🪜 Workflow kompakter

- Workflow-Hinweis deutlich flacher gestaltet.
- Schritte bleiben sichtbar, wirken aber nur noch als dezente Orientierung.
- Buch-Reiter ebenfalls kompakter gestaltet.

### 📄 Schreibblatt im Mittelpunkt

- Manuskripteditor stärker als ruhiges Schreibblatt gestaltet.
- Titelbereich und Formatierungsleiste verkleinert.
- Mehr horizontaler Platz für den eigentlichen Text.
- Zurückhaltendere Scrollbars.
- Kompaktere Statusleiste.

### 🗂️ Kapitel & Inspector

- Kapitel und Szenen bleiben kompakt und schnell anklickbar.
- Inspector optisch leichter und schmaler.
- Bei mittleren Bildschirmbreiten wird er weiter reduziert.
- Bei sehr engen Desktopbreiten wird er ausgeblendet, statt horizontalen Scroll zu erzeugen.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Autosave, Versionsschutz und Manuskriptinhalte unverändert.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.47.0

### ✒ Stabiles Scrollen im Manuskript

- Das Scrollverhalten des Schreibeditors wurde grundlegend beruhigt.
- Beim Scrollen eines längeren Kapitels können Buchkopf, Workflow-Leiste, Manuskript-Navigation und Editorwerkzeuge nicht mehr übereinander springen.
- Die Ursache waren mehrere gleichzeitig aktive `sticky`-Ebenen mit unterschiedlichen Höhen und Z-Indizes.
- Diese gestapelten Sticky-Bereiche wurden im Manuskript entfernt.

### 📄 Eigene Scrollfläche für den Kapiteltext

- Der eigentliche Kapiteltext scrollt jetzt innerhalb des Editors.
- Kapitel-/Szenenliste links und Inspector rechts bleiben dadurch stabil an ihrer Position.
- Editorüberschrift, Statusauswahl und Formatierungsleiste bleiben sauber oberhalb des Textes, ohne ihn zu überdecken.
- Die Statusleiste bleibt unterhalb des Editors erreichbar.
- Scrollbars erhalten stabilen Platz, damit sich die Breite beim Scrollen nicht verschiebt.

### 🧭 Manuskript-Arbeitsfläche

- Die dreispaltige Manuskriptansicht besitzt auf Desktop jetzt eine definierte, an die Fensterhöhe angepasste Arbeitsfläche.
- Lange Kapitel vergrößern nicht mehr die komplette Seite.
- Seitenleiste, Editor und Inspector verwenden innerhalb dieser Arbeitsfläche jeweils ihre passende eigene Scrolllogik.
- Auf kleinen Bildschirmbreiten fällt die Ansicht weiterhin auf normales Seiten-Scrolling zurück.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Änderung an Manuskriptinhalten, Autosave, Versionierung oder Sicherheitskopien.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.46.9

### ✒ Kompaktere Kapitel im Manuskript

- Kapitel und Szenen in der linken Manuskript-Navigation wachsen nicht mehr unnötig auf die verfügbare Höhe.
- Die Dokumentliste richtet ihre Zeilen jetzt am tatsächlichen Inhalt aus.
- Kapitel bleiben bewusst schmal und kompakt, damit man schnell durch viele Kapitel klicken kann.
- Wortzahl und Status bleiben weiterhin sichtbar.
- Szenen verwenden ebenfalls kompaktere Abstände.

### 🗂️ Rechtsklick-Menü für Kategorien

- Ein Rechtsklick auf eine Kategorie öffnet jetzt ein eigenes Kontextmenü.
- Verfügbar sind:
  - „Umbenennen“
  - „Neue Unterkategorie“
  - „Löschen“
- „Umbenennen“ verändert nur den Namen der Kategorie.
- „Neue Unterkategorie“ übernimmt die angeklickte Kategorie automatisch als übergeordneten Bereich.
- „Löschen“ verwendet weiterhin die bestehenden Sicherheitsregeln.
- Kategorie-Drag-&-Drop bleibt erhalten.

### 🎛️ Kontextmenü

- Das Menü öffnet sich an der Mausposition.
- Am Bildschirmrand bleibt es automatisch innerhalb des sichtbaren Bereichs.
- Klick außerhalb schließt das Menü wieder.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.46.8

### 🎨 Ruhigeres Farbsystem

- Dekorative Rot-/Rosatöne aus normalen Ideen- und Auswahlzuständen entfernt.
- Normale Zettel verwenden Creme, Salbei, Petrolblau und Flieder.
- Aktive Auswahl wird mit Petrol/Salbei hervorgehoben.
- Aktive Kategorien verwenden einen ruhigen Salbeiton.
- Dekorative Badges und UI-Akzente wurden entsprechend angepasst.
- Rot/Rosa bleibt für echte Warnungen, Fehler, Löschen und exakte Duplikate reserviert.
- Gelb/Ocker bleibt für „prüfen / möglicherweise ähnlich“.

### 🗂️ Kategorien bearbeiten & löschen besser auffindbar

- Jede Kategorie besitzt jetzt rechts einen Drei-Punkte-Button.
- Darüber kann die Kategorie direkt bearbeitet werden.
- Im Bearbeitungsdialog ist „🗑 Kategorie löschen“ klar sichtbar.
- Enthaltene Ideen werden beim Löschen weiterhin geschützt.
- Kategorien mit Unterkategorien müssen zunächst umsortiert werden, damit kein ganzer Zweig versehentlich verloren geht.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenbank- oder Backupänderung notwendig.
- Gefahren-/Fehlerfarben wurden absichtlich nicht überschrieben.

## v0.46.7

### ☑ Mehrfachauswahl mit Shift

- Ideen können jetzt wie in einer Dateiliste mit gedrückter Shift-Taste als Bereich markiert werden.
- Zuerst eine Idee auswählen, anschließend Shift gedrückt halten und eine weiter unten bzw. oben sichtbare Idee anklicken.
- Alle sichtbaren Ideen zwischen beiden Punkten werden zusätzlich markiert.
- Shift funktioniert sowohl über den Auswahlkreis als auch beim Klick auf den Zettel selbst.
- Strg/Cmd-Klick für einzelne zusätzliche Markierungen bleibt erhalten.

### ☑ Alle markieren

- In der unteren Auswahlleiste gibt es jetzt den Button „Alle markieren“.
- Der Button markiert sämtliche Ideen des aktuell gefilterten Ergebnisses.
- Das gilt für die aktuell gewählte Kategorie, Suche und aktive Filter.
- Auch Ideen auf weiteren Pagination-Seiten werden dabei mit markiert.
- Die Anzahl der aktuell markierbaren Ideen wird direkt im Button angezeigt.
- Sind bereits alle gefilterten Ideen markiert, zeigt der Button „Alle markiert“.

### 🧹 Auswahl zurücksetzen

- Beim Aufheben der Auswahl wird auch der Shift-Anker zurückgesetzt.
- Nach Verschieben oder Löschen einer Mehrfachauswahl wird der Auswahlanker ebenfalls sauber geleert.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Bestehende Auswahl-, Verschiebe-, Favoriten- und Papierkorb-Funktionen bleiben erhalten.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.46.6

### 🗂️ Kategorie-Drag & Drop repariert

- Die erste Drag-&-Drop-Umsetzung für Kategorien wurde ersetzt, weil sie im Browser zwar einen Greif-Cursor zeigte, das eigentliche Ziehen aber nicht zuverlässig auslöste.
- Kategorien verwenden jetzt einen eigenen Pointer-basierten Drag-Mechanismus.
- Eine Kategorie kann direkt am Namen bzw. an der gesamten Kategoriezeile angefasst und gezogen werden.
- Zusätzlich zeigt jede Kategorie einen kleinen Griff als visuelle Hilfe.
- Erst nach einer echten Mausbewegung wird der Drag-Modus aktiviert; ein normaler Klick öffnet die Kategorie weiterhin wie gewohnt.

### 🎯 Zielerkennung

- Beim Ziehen wird die Kategorie unter dem Mauszeiger als Ziel erkannt und sichtbar hervorgehoben.
- Beim Loslassen wird die gezogene Kategorie zu einer Unterkategorie des markierten Ziels.
- Die Dropzone „Auf Hauptebene verschieben“ bleibt erhalten.
- Nach dem Verschieben wird die Zielkategorie automatisch aufgeklappt.

### 🛡️ Schutz & bestehendes Ideen-Drag

- Eine Kategorie kann weiterhin weder auf sich selbst noch in einen eigenen Unterordner verschoben werden.
- Das Drag & Drop von Ideenzetteln auf Kategorien bleibt technisch getrennt erhalten.
- Ein unmittelbar nach dem Ziehen ausgelöster Klick wird unterdrückt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.46.5

### 🗂️ Kategorien per Drag & Drop verschieben

- Kategorien können jetzt direkt im Kategorienbaum per Drag & Drop verschoben werden.
- Wird eine Kategorie auf eine andere Kategorie gezogen, wird sie zu deren Unterkategorie.
- Beispiel: „Glaube & Religion“ kann direkt auf „Gesellschaft“ gezogen werden.
- Enthaltene Ideen und Unterkategorien bleiben vollständig erhalten.
- Die Zielkategorie wird nach dem Verschieben automatisch aufgeklappt.

### ↥ Zurück auf die Hauptebene

- Während eine Kategorie gezogen wird, erscheint die Dropzone „Auf Hauptebene verschieben“.
- Eine Unterkategorie kann dort abgelegt werden, um wieder zu einer Hauptkategorie zu werden.

### 🛡️ Schutzregeln

- Keine Kategorie kann auf sich selbst gezogen werden.
- Keine Kategorie kann in einen ihrer eigenen Unterordner verschoben werden.
- Ungültige Dropziele werden nicht akzeptiert.
- Das Drag & Drop von Ideenzetteln bleibt unverändert erhalten.

### 🎛️ Bedienung

- Gezogene Kategorien werden optisch abgeschwächt.
- Gültige Zielkategorien werden hervorgehoben.
- Kategoriezeilen zeigen einen Greif-Cursor.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Kategoriehierarchie und Reihenfolge werden nach dem Verschieben gespeichert.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.46.4

### 🗂️ Kategorien wieder direkt anklickbar

- Ein Fehler in der Navigation zwischen Dashboard und Ideenarchiv wurde behoben.
- Beim Klick auf eine Kategorie wurde zwar intern die Kategorie gewechselt, die Oberfläche blieb jedoch auf dem Dashboard.
- Dadurch änderten sich Breadcrumbs und Zähler, ohne dass die eigentliche Ideenansicht erschien.
- Ein Klick auf eine Kategorie wechselt jetzt zuverlässig in das Ideenarchiv und zeigt direkt die gewählte Kategorie.
- Das Auf- und Zuklappen über den kleinen Pfeil bleibt davon getrennt.
- Kategorien können zusätzlich per Tastatur mit Enter oder Leertaste geöffnet werden.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Kategorienavigation gezielt auf den Wechsel Dashboard → Ideenarchiv geprüft.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.46.3

### 🗂️ Massenimport – eindeutige Statusfarben

- Zufällig wechselnde Zettelfarben aus der Importvorschau entfernt.
- Farben besitzen jetzt ausschließlich eine inhaltliche Bedeutung.
- Grün/neutral = neue Idee.
- Gelb/Ocker = möglicherweise ähnliche Idee.
- Rosa/Rot = exakt vorhandenes Duplikat.
- Textstatus bleibt zusätzlich sichtbar.

### 🔤 Bessere Lesbarkeit beim Import

- Schreibmaschinen-/Courier-Schrift aus der Importvorschau entfernt.
- Klarere Sans-Serif-Schrift für längere Ideentexte.
- Zeilenabstand verbessert.
- Eingabefeld und Vorschau typografisch vereinheitlicht.
- Duplikaterkennungslogik unverändert.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.46.2

### 📚 Hoverkarte am Bücherregal korrigiert

- Die Buch-Infokarte bleibt jetzt innerhalb der sichtbaren Regalfläche.
- Bei Büchern am linken Rand wird die Karte automatisch nach rechts verschoben.
- Bei Büchern am rechten Rand wird sie entsprechend nach links verschoben.
- Die Pfeilspitze der Infokarte bleibt optisch auf den jeweiligen Buchrücken ausgerichtet.
- Die Position wird beim Hover bzw. Tastaturfokus anhand der tatsächlichen Regalbreite berechnet.
- Die Buchrücken selbst bleiben unverändert direkt aneinander stehen.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine Datenbank- oder Backupänderung notwendig.

## v0.46.0

### 📚 Cover-Bücher im Regal

- Bücher mit Cover werden jetzt als gerade stehende Bücher dargestellt.
- Das Cover füllt die sichtbare Vorderseite vollständig aus.
- Ein schmaler echter Buchrücken wird links neben dem Cover dargestellt.
- Bücher kippen weder im normalen Zustand noch beim Hover.
- Bücher ohne Cover verwenden weiterhin die Buchrückenansicht, ebenfalls ohne Schrägstellung.

### 🎨 Buchrückenfarbe automatisch aus dem Cover

- Neue Option „Automatisch aus dem Cover übernehmen“.
- Beim Auswählen eines Covers wird lokal eine dominante Bildfarbe ermittelt.
- Sehr helle, sehr dunkle und nahezu graue Bildbereiche werden bei der Ermittlung reduziert gewichtet.
- Die gefundene Farbe wird als Buchrückenfarbe eingesetzt.
- Manuelle Änderungen bleiben weiterhin möglich und deaktivieren die Automatik.
- Die Farbanalyse erfolgt vollständig lokal im Browser.

### 📁 Ordner & Unterordner im Anhang

- Anhänge können jetzt eigene Ordnerstrukturen besitzen.
- Ordner können auf der Wurzelebene angelegt werden.
- Jeder Ordner kann weitere Unterordner besitzen.
- Eine Breadcrumb-Navigation zeigt den aktuellen Pfad.
- Story-Bibel-Einträge können direkt einem Ordner oder Unterordner zugewiesen werden.
- Bestehende Einträge bleiben unverändert auf der Wurzelebene.

### 🎨 Ideenstatistik

- „+X dieses Jahr“ verwendet jetzt dieselbe grüne Wachstumsfarbe wie Woche und Monat.
- Die bisherige rote Darstellung stammte lediglich aus einer alten CSS-Akzentregel und hatte keine inhaltliche Bedeutung.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Keine fehlenden festen UI-Verweise.
- Keine doppelten HTML-IDs.
- Keine doppelten JavaScript-Funktionen.
- Keine Datenbankmigration notwendig.

## v0.45.5

### 🛡️ Data Guard & Recovery

- Ein scheinbar leeres Archiv wird nicht mehr automatisch als normaler Zustand akzeptiert.
- Die Startsequenz lädt Daten zunächst ausschließlich lesend.
- Ladefehler werden gesammelt und sichtbar gemacht.
- Ein lokales Datenmanifest merkt sich die zuletzt bekannte Anzahl zentraler Datensätze.
- Unerwartet leere oder deutlich reduzierte Datenbestände aktivieren eine Schreibsperre.
- Bei lokaler `file://`-Nutzung wird ausdrücklich auf die mögliche Pfadabhängigkeit des Browser-Speichers hingewiesen.
- Neue leere lokale Speicherorte müssen bewusst als neues Archiv bestätigt werden.
- Ein Notfall-Rohdatenexport kann direkt aus dem Schutzdialog erstellt werden.
- Automatische Bereinigungen und Migrationen laufen erst nach einer erfolgreichen Startprüfung.
- Verdächtige Ladezustände können nicht per Escape übergangen werden.
- Datenbank- und reguläres Backupformat bleiben unverändert.

## v0.45.3

### 📚 Bücher & Romanprojekte zusammengeführt

- „Romanprojekte“ und „Bücher“ sind in der sichtbaren Hauptnavigation jetzt ein gemeinsamer Bereich.
- Der neue Menüpunkt heißt „Bücher & Projekte“.
- Die interne Datenstruktur bleibt bewusst getrennt:
  - Romanprojekt = übergeordnete Welt, Planung und gemeinsame Daten
  - Buch = einzelner Band mit eigenem Manuskript
- Dadurch bleibt die saubere Architektur erhalten, ohne dass sich die Benutzeroberfläche wie zwei getrennte Verwaltungsbereiche anfühlt.

### 🗂️ Projekte direkt im Bücherregal

- Bücher werden weiterhin nach Romanprojekt gruppiert.
- Der Projektname in jeder Gruppe ist jetzt direkt anklickbar und öffnet den vollständigen Projektarbeitsbereich.
- Direkt an jeder Projektgruppe stehen Aktionen zum Bearbeiten des Projekts und zum Anlegen eines neuen Buches innerhalb dieses Projekts bereit.
- Ein neues Romanprojekt kann direkt aus „Bücher & Projekte“ angelegt werden.
- Nach dem Anlegen eines Projekts bleibt die Nutzerin im gemeinsamen Bücher-/Projektbereich.

### 🧭 Navigation

- Frühere interne Verweise auf den separaten Romanprojekt-Hauptbereich führen jetzt in „Bücher & Projekte“.
- Es gibt dadurch keinen doppelten Hauptnavigationspunkt mehr.
- Projekt-Workspaces selbst bleiben vollständig erhalten und können direkt aus dem Bücherregal geöffnet werden.

### ✓ Architektur

- Keine Datenmigration notwendig.
- Projekt- und Buchdatensätze bleiben intern getrennt und sauber referenziert.
- Backup- und Datenbankformat bleiben unverändert.

## v0.45.2

### 🧭 Hauptnavigation nochmals gehärtet

- Die untere Hauptnavigation verwendet jetzt nur noch eine zentrale Event-Delegation direkt auf der Navigationsleiste.
- Das Routing wird bereits vor den übrigen Modul-Eventbindungen installiert.
- Dadurch bleiben Dashboard, Ideenarchiv, Romanideen, Romanprojekte, Bücher, Namenslisten, Recherche, Inspiration, Mindmaps, Statistiken und Einstellungen auch dann anklickbar, wenn in einem später geladenen Modul ein separater Event-Handler fehlschlagen sollte.
- Die Navigationsleiste besitzt jetzt ausdrücklich eigene Pointer-Events, einen höheren Ebenenwert und eine isolierte Stacking-Ebene, damit Arbeitsbereiche keine Klicks überdecken können.

### × Dialog-Schließen repariert

- Das Schließen-X von Formular-Dialogen ist jetzt technisch kein Submit-Button mehr.
- Dadurch löst das X keine HTML-Formularvalidierung aus.
- Insbesondere kann „Neue Idee“ jetzt auch mit leerem Pflichtfeld über das X geschlossen werden.
- Das X wird zentral über den umgebenden Dialog behandelt und schließt ihn unabhängig von Pflichtfeldern.
- Der Button „Abbrechen“ im Dialog „Neue Idee“ wurde ebenfalls auf einen echten nicht-validierenden Abbruch umgestellt.
- Escape bleibt weiterhin verwendbar.

### ⌂ Startansicht

- Das Dashboard bleibt weiterhin die feste Startansicht beim Öffnen der Suite.
- Mindmaps bleiben sauber als eigener Hauptbereich getrennt.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Zentrale Navigation auf genau einen Routingpfad reduziert.
- Dialog-Schließen unabhängig von Formularvalidierung geprüft.
- Keine Änderung am Datenbank- oder Backupformat erforderlich.

## v0.45.1

### 🧭 Navigation & Startansicht

- Die untere Hauptnavigation wurde zentralisiert und repariert.
- Dashboard, Ideenarchiv, Romanideen, Romanprojekte, Bücher, Namenslisten, Recherche, Inspiration, Mindmaps, Statistiken und Einstellungen wechseln jetzt wieder eindeutig zwischen den jeweiligen Arbeitsbereichen.
- Doppelte Mindmap-Routen in der Navigation wurden entfernt.
- Die kleine Suite-Navigation verwendet jetzt dieselbe zentrale Routing-Logik wie die untere Hauptnavigation.

### ⌂ Dashboard als Start

- Beim frischen Öffnen der Autoren-Suite erscheint jetzt immer zuerst das Dashboard.
- Die zuletzt verwendete Mindmap oder das Ideenarchiv wird nicht mehr automatisch direkt beim Programmstart geöffnet.
- Von dort kann wie gewohnt in jeden Arbeitsbereich gewechselt werden.

### ⌘ Mindmaps sauber getrennt

- Mindmaps werden nicht mehr unterhalb des Ideenarchivs sichtbar.
- Ursache war eine CSS-Regel des Mindmap-Arbeitsbereichs, die das HTML-Attribut `hidden` optisch überschreiben konnte.
- Ausgeblendete Arbeitsbereiche werden jetzt global zuverlässig mit `display: none` verborgen.
- Dadurch kann immer nur der aktuell gewählte Hauptbereich sichtbar sein.

### ✓ Technische Prüfung

- JavaScript-Syntax geprüft.
- Zentrale Navigationsziele geprüft.
- Startlogik auf Dashboard umgestellt.
- Sichtbarkeitslogik für Arbeitsbereiche gehärtet.
- Datenbank- und Backupformat bleiben unverändert; es handelt sich um einen reinen UI-/Navigationsfix.

## v0.45.0

### ⚙ Architecture & Reliability

Diese Version konzentriert sich ausschließlich auf den technischen Unterbau der Autoren-Suite. Es wurden keine neuen Schreibfunktionen ergänzt. Ziel ist ein belastbarerer Sockel für den langfristigen Einsatz und spätere Erweiterungen.

### 💾 Zuverlässigere IndexedDB-Schreibvorgänge

- Die zentralen Schreiboperationen der Suite wurden gehärtet.
- Speichern, Anlegen und Löschen warten jetzt auf den erfolgreichen Abschluss der vollständigen IndexedDB-Transaktion und nicht nur auf die Antwort des einzelnen Requests.
- Zentrale Schreibvorgänge laufen über eine gemeinsame Schreibwarteschlange.
- Dadurch werden konkurrierende Schreiboperationen kontrollierter nacheinander abgearbeitet.
- Fehler und abgebrochene Transaktionen werden zentral an die vorhandene Speicherfehlerbehandlung weitergereicht.
- Auch das Ideenarchiv wurde in diese zuverlässigere Schreibkette aufgenommen.

### 🧱 Datenbank-Lebenszyklus

- Die IndexedDB-Verbindung reagiert jetzt explizit auf Versionswechsel aus einem anderen Browser-Tab.
- Veraltete Datenbankverbindungen werden geschlossen.
- Die Oberfläche weist anschließend auf das notwendige Neuladen hin.
- Blockierte Datenbank-Upgrades werden sichtbar gemeldet.

### 🛟 Sichererer Backup-Import

- Vor einer vollständigen Wiederherstellung wird intern zuerst ein Snapshot des aktuellen Archivs erzeugt.
- Schlägt die Wiederherstellung nach dem Löschen des bisherigen Datenbestands fehl, versucht die Suite automatisch, den vorherigen Zustand zurückzuspielen.
- Diese automatische Rücksicherung ist eine zusätzliche Schutzschicht und ersetzt kein externes Backup.
- Relationale IDs bleiben bei einer vollständigen Wiederherstellung erhalten.

### 🔍 Strengere Backup-Prüfung

- Die Backup-Prüfung kontrolliert doppelte IDs jetzt in sämtlichen relationalen Modulbereichen.
- Beschädigte oder unvollständige Datenbereiche werden vor dem eigentlichen Import abgewiesen.
- Das Backupformat wurde auf Version 10 erweitert.
- Ältere unterstützte Backupstände bleiben importierbar.

### 🧪 Tiefenprüfung

- Der Sicherheitsbereich besitzt jetzt eine zusätzliche technische Tiefenprüfung.
- Dabei wird ein echter kurzer IndexedDB-Schreib-/Löschzyklus durchgeführt.
- Zusätzlich wird die Grundstruktur der geladenen Daten geprüft.
- Speicherverbrauch und verfügbares Speicherkontingent werden angezeigt, sofern der Browser diese Informationen bereitstellt.
- Der Testeintrag wird unmittelbar wieder entfernt.

### ⌂ Dauerhafter Browser-Speicher

- Die Suite kann den Browser jetzt um persistenten lokalen Speicher bitten.
- Wird dies gewährt, sinkt das Risiko einer automatischen Bereinigung der lokalen Anwendungsdaten.
- Die Entscheidung darüber liegt beim Browser.
- Externe Backups bleiben weiterhin wichtig.

### 🧩 Erweiterter Systemcheck

- Zusätzlich geprüft werden:
  - Mindmap-Knoten ohne vorhandene Mindmap
  - Mindmap-Verbindungen ohne vorhandene Mindmap
  - Mindmap-Knoten mit fehlenden verknüpften Suite-Inhalten
  - Handlungsfäden mit fehlenden Buchverknüpfungen
  - Handlungsfaden-Stationen mit fehlenden Manuskriptabschnitten

### 🧭 Architekturentscheidung

- Das IndexedDB-Datenmodell bleibt auf Schema-Version 8.
- Für diese Zuverlässigkeitsrunde war keine unnötige Datenmigration erforderlich.
- Neue Schutzschichten wurden um die bestehenden Stores gelegt, statt funktionierende Datenstrukturen ohne fachlichen Grund umzubauen.
- Die Suite bleibt vollständig lokal und benötigt keinen Server oder Account.

### ✓ Technische Prüfung

- JavaScript-Syntax, feste UI-Verweise, HTML-IDs und Funktionsnamen wurden erneut statisch geprüft.
- Die Reliability-Funktionen wurden in die bestehende Sicherheitsoberfläche integriert.
- Die Backup-Abwärtskompatibilität bleibt erhalten.

## v0.44.0

### ✦ Große Workflow-, UI- & Sicherheitsrunde

Diese Version führt bewusst kein neues großes Inhaltsmodul ein. Stattdessen wurde die inzwischen umfangreiche Autoren-Suite als zusammenhängendes Arbeitsprogramm überprüft, geglättet und insbesondere beim Schreiben weiter abgesichert.

### ⌁ Workflow sichtbar gemacht

- Im Bucharbeitsbereich gibt es jetzt eine kompakte Workflow-Leiste.
- Sie bildet den Weg von Ideen über Planung und Entwurf bis Story-Bibel und Verfeinerung sichtbar ab.
- Die Leiste ist ausdrücklich keine Pflichtreihenfolge.
- Je nach vorhandenem Material zeigt sie einen passenden nächsten Hinweis.
- Die einzelnen Workflow-Schritte sind direkt anklickbar.

### 🗺️ Zentrale Workflow-Karte

- Über „Workflow“ im Schreibbereich lässt sich eine vollständige Karte der Suite öffnen.
- Sie zeigt die Verzahnung von Mindmaps, Ideenarchiv, Romanideen, Romanprojekten, Büchern, Planung, Manuskript, Story-Bibel, Handlungsfäden, Timeline und Beziehungen.
- Von der Karte kann direkt in die jeweiligen Bereiche gesprungen werden.

### ✒️ Schreibsicherheit weiter verstärkt

- Der bestehende Notfallschutz für offene Manuskripttexte wurde erweitert.
- Der aktuelle Entwurf wird parallel in zwei getrennten Browser-Speichern gespiegelt, soweit beide verfügbar sind.
- Ein zusätzlicher regulärer Speicherlauf erfolgt während des Schreibens alle 30 Sekunden.
- Die schnelle Notfallkopie wird ungefähr alle vier Sekunden aktualisiert.
- Beim Wechsel aus dem Manuskript in einen anderen Buchbereich wird vorsorglich gespeichert.
- Der Schutz beim Verbergen, Verlassen und Schließen der Seite bleibt aktiv.

### ↶ Versionsstände

- Automatische Sicherheitsversionen werden nun ungefähr alle drei Minuten bei Änderungen angelegt.
- Vor der Wiederherstellung einer älteren Version wird der aktuelle Stand weiterhin zuerst gesichert.

### ◷ Chronologie-Sicherheitsfix

- Eine Lücke aus dem Timeline-Ausbau wurde geschlossen.
- Datum, Uhrzeit, relative Story-Zeit, Handlungsstrang und Dauer werden jetzt auch im primären Manuskript-Autosave vollständig berücksichtigt.

### ✓ Schnellzugriff Sicherheit

- Im Schreibbereich gibt es jetzt einen direkten Sicherheitsbutton.
- Probleme mit dem lokalen Schreibschutz werden dort sichtbar signalisiert.

### ⌨️ Sicherheits-Shortcuts

- `Strg/Cmd + Shift + S` aktualisiert unmittelbar den aktuellen Sicherheitsstand.
- `Strg/Cmd + Shift + W` öffnet die Workflow-Karte.

### 🎛️ UI-Hierarchie

- Der umfangreiche Buchworkflow wurde visuell neu gegliedert.
- Die Workflow-Navigation bleibt beim Scrollen leichter erreichbar.
- Große Arbeitsbereiche besitzen ruhigere maximale Breiten.
- Dialogaktionen bleiben bei langen Formularen besser erreichbar.
- Schmale und mobile Ansichten wurden nachgezogen.

### 💾 Backup-Kompatibilität

- Das vollständige Backupformat wurde auf Version 9 erweitert.
- v0.43/v8-Backups und ältere unterstützte Backupstände bleiben importierbar.
- Sämtliche bestehenden Inhalte einschließlich Mindmaps, Beziehungsnetzen, Timeline und Story-Bibel bleiben Bestandteil des Backups.

### ✓ Technische Prüfung

- Die Oberfläche wurde auf doppelte IDs, fehlende feste UI-Verweise und doppelte JavaScript-Funktionen geprüft.
- Die Manuskript-Sicherheitskette wurde gezielt auf Editor, Inspector, Chronologie, Versionsstände und Notfallkopien geprüft.
- Bestehende Datenmodelle wurden nicht unnötig umgebaut.

## v0.43.0

### ⌘ Beziehungen, Netzwerke & Mindmaps

- Die Autoren-Suite besitzt jetzt einen eigenständigen Mindmap-Bereich und ein Story-Bibel-Beziehungsnetz.
- Beide Systeme verwenden verknüpfbare Knoten, bleiben aber bewusst fachlich getrennt:
  - Mindmaps dienen dem freien Denken und Sortieren.
  - Beziehungsnetze visualisieren tatsächliche Zusammenhänge innerhalb einer Geschichte.

### 🧠 Freie Mindmaps

- Es können beliebig viele Mindmaps angelegt werden.
- Eine Mindmap darf vollständig unabhängig von einem Romanprojekt oder Buch existieren.
- Optional kann sie später mit einem Romanprojekt und/oder Buch verbunden werden.
- Dadurch können neue Stoffe zunächst völlig frei entstehen, bevor feststeht, zu welchem Projekt sie gehören.
- Jede Mindmap besitzt Titel und Beschreibung.

### 🗒️ Frei positionierbare Gedanken

- Neue Gedanken werden als frei verschiebbare Karten auf einer großen Arbeitsfläche angelegt.
- Karten können per Drag & Drop beliebig positioniert werden.
- Für visuelle Gruppen stehen mehrere Papierfarben zur Verfügung.
- Jeder Knoten kann einen Titel und eine ausführlichere Notiz enthalten.
- Die Mindmap-Arbeitsfläche unterstützt Zoom.
- Mit „Alles zeigen“ kann die Ansicht auf eine übersichtliche Grundposition zurückgesetzt werden.

### 🔗 Gedanken miteinander verbinden

- Zwei Mindmap-Knoten können direkt miteinander verbunden werden.
- Verbindungen können frei beschriftet werden.
- Unterstützt werden normale, gestrichelte und gerichtete Verbindungen.
- Bestehende Verbindungen können geöffnet, bearbeitet und gelöscht werden.

### ↗ Suite-Inhalte als echte Knoten

- Mindmap-Knoten können direkt auf bestehende Inhalte der Autoren-Suite verweisen.
- Unterstützt werden Ideen, Romanideen, Romanprojekte, Bücher, Story-Bibel-Akten, Kapitel und Szenen, Handlungsfäden, Timeline-Ereignisse, Recherche und Inspiration.
- Der Knoten bleibt Bestandteil der Mindmap, verweist aber auf den echten Inhalt statt eine zweite Kopie anzulegen.

### → Vom losen Gedanken zur echten Idee

- Freie Mindmap-Knoten können direkt als Idee ins Ideenarchiv übernommen werden.
- Titel und Notiz werden dabei zu einem Ideenzettel zusammengeführt.
- Der ursprüngliche Mindmap-Knoten wird anschließend automatisch mit der neu angelegten Idee verknüpft.

### ↔ Story-Bibel-Beziehungen

- Bücher besitzen jetzt einen eigenen Workflow-Bereich „Beziehungen“.
- Story-Bibel-Akten eines Buches werden als visuelles Beziehungsnetz dargestellt.
- Figuren, Orte, Organisationen, Gegenstände, Ereignisse und freie Akten können miteinander verbunden werden.
- Beziehungen besitzen eine frei definierbare Bezeichnung, Richtung, optionalen Status, optionale Notiz und optionalen Gültigkeitsbezug zu einem bestimmten Buch.

### 🕸️ Visuelles Beziehungsnetz

- Story-Bibel-Akten werden als farblich nach Typ unterscheidbare Knoten dargestellt.
- Beziehungslinien werden automatisch zwischen den Akten gezeichnet.
- Beschriftungen erscheinen direkt an den Verbindungen.
- Gerichtete Beziehungen werden visuell von gegenseitigen Beziehungen unterschieden.
- Ein Fokusfilter erlaubt es, nur das Umfeld einer bestimmten Figur, eines Ortes oder einer anderen Akte zu betrachten.

### 📚 Beziehungen über Reihen hinweg

- Beziehungen können entweder für die gesamte Story gelten oder auf einen konkreten Band begrenzt werden.
- Für die Bandauswahl werden die bereits vorhandenen Reihen- und Buchverknüpfungen verwendet.

### 🔎 Globale Suche

- Mindmaps, Mindmap-Knoten und Story-Beziehungen sind Bestandteil der globalen Suche.
- Treffer führen direkt zum jeweiligen Arbeitsbereich.

### 💾 Datenmodell & Backup

- IndexedDB wurde um Mindmaps, Mindmap-Knoten, Mindmap-Verbindungen und Story-Beziehungen erweitert.
- Das Datenbankschema wurde auf Version 8 erweitert.
- Das vollständige Backupformat wurde auf Version 8 erweitert.
- Ältere unterstützte Backups bleiben importierbar.
- Sämtliche Daten bleiben vollständig lokal im Browser.

### ✓ Systemcheck

- Der Systemcheck erkennt Mindmap-Verbindungen, deren Knoten nicht mehr vorhanden sind.
- Story-Beziehungen zu fehlenden Story-Bibel-Akten werden ebenfalls diagnostisch gemeldet.
- Es werden keine Beziehungen oder Mindmap-Daten automatisch verändert.

## v0.42.0

### ◷ Story-Timeline & Chronologie

- Jedes Buch besitzt jetzt einen eigenen Bereich „Timeline“.
- Die Timeline trennt bewusst die Reihenfolge im Manuskript von der tatsächlichen Chronologie innerhalb der Geschichte.
- Konkrete Daten und Uhrzeiten können mit unscharfen oder relativen Zeitangaben kombiniert werden.
- Die Timeline kann wahlweise nur das aktuelle Buch oder die komplette verbundene Reihe bzw. das Universum anzeigen.

### ✒️ Szenen zeitlich einordnen

- Kapitel und Szenen besitzen jetzt zusätzliche Chronologie-Felder.
- Hinterlegt werden können:
  - Datum
  - Uhrzeit
  - relative bzw. unscharfe Zeit
  - Handlungsstrang
  - Dauer
- Die bisherige freie Angabe „Zeit / Einordnung“ bleibt erhalten und kann parallel weiterverwendet werden.
- Chronologie-Angaben werden direkt im Szenen-Inspector gepflegt und automatisch mit dem Abschnitt gespeichert.

### 🌫️ Unscharfe & relative Zeit

- Geschichten benötigen nicht zwingend echte Kalenderdaten.
- Angaben wie „Frühling“, „Tag 17“, „drei Tage später“, „zehn Jahre zuvor“ oder andere freie Formulierungen können direkt verwendet werden.
- Undatierte Timeline-Punkte können per Drag & Drop in eine relative Reihenfolge gebracht werden.
- Die Veränderung der Story-Chronologie ändert dabei nicht automatisch die Reihenfolge des Manuskripts.

### 🧱 Manuskript vs. Story-Reihenfolge

- Manuskriptabschnitte zeigen auf der Timeline zusätzlich ihre Position im eigentlichen Manuskript.
- Weicht die chronologische Position deutlich von der Manuskriptreihenfolge ab, wird dies als möglicher Zeitsprung bzw. Rückblenden-Hinweis sichtbar gemacht.
- Die Suite verändert die Manuskriptstruktur dabei niemals automatisch.

### ➕ Freie Timeline-Ereignisse

- Unabhängig vom Manuskript können eigene Timeline-Ereignisse angelegt werden.
- Freie Ereignisse besitzen Titel, Beschreibung, Datum, Uhrzeit, relative Zeit, Handlungsstrang und Ort.
- Ein Ereignis kann mit einem oder mehreren Büchern verknüpft werden.
- Story-Bibel-Akten können zusätzlich direkt mit einem Timeline-Ereignis verbunden werden.
- Dadurch lassen sich beispielsweise Geburten, Umzüge, Kriege, Gründungen, historische Ereignisse oder Geschehnisse außerhalb des eigentlichen Manuskripts dokumentieren.

### 📎 Story-Bibel & Chronologie

- Story-Bibel-Einträge besitzen jetzt optionale Chronologie-Felder.
- Besonders Ereignis-Akten können dadurch direkt auf der Timeline erscheinen.
- Auch Figuren-, Orts- oder Weltakten können bei Bedarf zeitlich eingeordnet werden.
- Unterstützt werden Datum, Uhrzeit, relative Zeit und ein Handlungsstrang.
- Die bereits vorhandenen bandspezifischen Kontinuitätsstände bleiben davon unabhängig erhalten.

### 🧵 Handlungsfäden auf der Timeline

- Stationen aus Handlungsfäden können auf der Timeline erscheinen, wenn sie mit einer zeitlich eingeordneten Szene verknüpft sind.
- Dadurch werden Einführung, Entwicklung und Auflösung eines Geheimnisses oder Konflikts im zeitlichen Verlauf der Geschichte sichtbar.
- Handlungsfäden bleiben weiterhin in ihrem eigenen Bereich vollständig bearbeitbar.

### 👤 Figurenfilter

- Die Timeline kann nach Story-Bibel-Figuren gefiltert werden.
- Dadurch lässt sich gezielt verfolgen, welche zeitlich eingeordneten Szenen und Ereignisse mit einer bestimmten Figur verbunden sind.
- Voraussetzung ist eine vorhandene Verknüpfung des Timeline-Punkts mit der entsprechenden Figurenakte.

### 🛤️ Handlungsstränge & Parallelhandlungen

- Timeline-Punkte können einem frei benannten Handlungsstrang zugeordnet werden.
- Die Timeline kann anschließend gezielt nach einem Handlungsstrang gefiltert werden.
- Dadurch lassen sich parallele Erzählstränge, Rückblenden, unterschiedliche Figurenperspektiven oder historische Ebenen voneinander trennen.

### ⚠️ Dezente Kontinuitätshinweise

- Die Timeline erkennt einfache mögliche Zeitüberschneidungen.
- Wenn dieselbe verknüpfte Figur zur exakt gleichen Zeit an unterschiedlichen hinterlegten Orten auftaucht, wird ein Hinweis angezeigt.
- Die Suite entscheidet nicht selbstständig, dass ein Fehler vorliegt.
- Hinweise bleiben rein diagnostisch und verändern keine Story-Daten.

### ✦ Reihe & Universum

- Die Timeline kann auf „Reihe / Universum“ umgeschaltet werden.
- Dafür werden die bereits vorhandenen Buchverknüpfungen und Reihendaten verwendet.
- Zeitlich eingeordnete Szenen, Story-Bibel-Ereignisse, freie Timeline-Ereignisse und Handlungsfaden-Stationen aus mehreren Bänden können gemeinsam betrachtet werden.
- Es entsteht keine zweite getrennte Reihenverwaltung.

### ⌂ Integration in die Buchübersicht

- Bücher mit vorhandenen Timeline-Punkten zeigen auf der Buchübersicht einen direkten Hinweis auf ihre Chronologie.
- Von dort kann direkt in die Timeline gewechselt werden.

### 🔎 Globale Suche

- Freie Timeline-Ereignisse sind Bestandteil der globalen Suche.
- Durchsucht werden Titel, Beschreibung, Datum, relative Zeit, Handlungsstrang, Ort, verknüpfte Bücher und verknüpfte Story-Bibel-Akten.
- Suchtreffer führen direkt in die Timeline und öffnen bei Bedarf das entsprechende Ereignis.

### 🛠️ Universumskarte

- Die Ermittlung verbundener Bücher aus der Universumskarte wurde korrigiert und gehärtet.
- Direkte Buchverknüpfungen werden jetzt zuverlässig über die vorhandenen Gegenbeziehungen verfolgt.
- Hinterlegte Cover verwenden wieder konsistent das tatsächliche Coverfeld des Buches.

### 💾 Datenmodell & Backup

- IndexedDB wurde um einen eigenen Speicherbereich für freie Timeline-Ereignisse erweitert.
- Das Datenbankschema wurde auf Version 7 erweitert.
- Das vollständige Backupformat wurde auf Version 7 erweitert.
- v0.42-Backups enthalten Timeline-Ereignisse und alle neuen Chronologie-Felder.
- Backups aus v0.41/v0.40 sowie ältere v6-, v5- und v4-Backups bleiben weiterhin importierbar.
- Sämtliche Chronologie-Daten bleiben vollständig lokal im Browser.

### ✓ Systemcheck

- Der Systemcheck prüft jetzt Timeline-Ereignisse auf Verknüpfungen zu nicht mehr vorhandenen Büchern.
- Auch fehlende Story-Bibel-Akten innerhalb von Timeline-Ereignissen werden als Hinweis erkannt.
- Die Prüfung bleibt rein diagnostisch und verändert keine Timeline-Daten automatisch.

## v0.41.0

### ✦ Reihen- & Universumskarte

- Jedes Buch besitzt jetzt einen eigenen Bereich „Universum“.
- Die Ansicht führt verbundene Bücher, Reiheninformationen, gemeinsame Story-Bibel-Akten und bandübergreifende Handlungsfäden an einer Stelle zusammen.
- Bereits vorhandene Buchverknüpfungen werden automatisch für die Karte verwendet.
- Bücher mit demselben Reihennamen werden zusätzlich gemeinsam dargestellt, auch wenn noch nicht jede Verbindung einzeln angelegt wurde.

### 📚 Visuelle Buchkarte

- Verbundene Bücher werden als horizontale Buchkarte dargestellt.
- Vorhandene Cover werden direkt verwendet.
- Das aktuell geöffnete Buch ist optisch hervorgehoben.
- Zu jedem Buch werden Status, Reihe sowie Hinweise auf offene Handlungsfäden und Kontinuitätsnotizen angezeigt.
- Ein Klick auf ein Buch öffnet direkt dessen Arbeitsbereich.
- Neue Buchverknüpfungen können direkt aus der Universumskarte angelegt werden.

### 🔗 Buchbeziehungen

- Die Universumskarte besitzt eine eigene Übersicht aller direkten Verbindungen zwischen den dargestellten Büchern.
- Beziehungstyp und vorhandene Notizen werden kompakt angezeigt.
- Bestehende Verbindungen können direkt aus der Karte geöffnet und bearbeitet werden.

### 📎 Gemeinsame Story-Bibel

- Die Karte erkennt Story-Bibel-Einträge, die für mehrere Bücher des dargestellten Universums gelten.
- Figuren, Orte, Weltwissen und andere gemeinsame Akten werden gesammelt angezeigt.
- Angezeigt wird, für welche Bücher eine Akte gilt.
- Ein Klick öffnet direkt die vollständige Story-Bibel-Akte.

### 🧵 Bandübergreifende Handlungsfäden

- Handlungsfäden, die mehrere Bücher verbinden, werden in einer eigenen Reihenansicht dargestellt.
- Sichtbar sind Typ, Titel, beteiligte Bücher und aktueller Status.
- Dadurch werden beispielsweise Geheimnisse oder Foreshadowing sichtbar, die in Band 1 beginnen und erst in einem späteren Band aufgelöst werden.
- Ein Klick öffnet den vollständigen Handlungsfaden.

### ⌂ Integration in den Buch-Workflow

- Die Universumskarte ist als eigener Workflow-Tab direkt in jedem Buch erreichbar.
- Zusätzlich besitzt die Buchübersicht einen direkten Sprung in den Universumsbereich.
- Die Ansicht verwendet die bereits vorhandenen Daten und erzeugt keine getrennte zweite Reihenverwaltung.

## v0.40.0

### 🧵 Kontinuität & Handlungsfäden

- Die Autoren-Suite besitzt jetzt einen eigenen buch- und reihenübergreifenden Bereich für offene Handlungsfäden.
- Unterstützte Fadentypen sind Geheimnis, Foreshadowing, Konflikt, Versprechen, offene Frage, Beziehung, Hinweis/Gegenstand und freie Typen.
- Jeder Faden besitzt einen Status: offen, in Entwicklung, aufgelöst oder fallengelassen.
- Handlungsfäden können mit mehreren Büchern und mehreren Story-Bibel-Akten gleichzeitig verbunden werden.

### 🗺️ Verlauf eines Handlungsfadens

- Für jeden Faden können beliebig viele Stationen angelegt werden.
- Stationen können als Einführung, Wiederaufnahme, Entwicklung oder Auflösung markiert werden.
- Jede Station kann einem Buch und optional einem konkreten Kapitel bzw. einer Szene zugeordnet werden.
- Dadurch lässt sich nachvollziehen, wo ein Geheimnis eingeführt, später wieder aufgegriffen und schließlich aufgelöst wird.

### 📌 Kontinuitätsstand pro Buch

- Story-Bibel-Einträge besitzen jetzt zusätzlich zur gemeinsamen Hauptakte bandspezifische Kontinuitätsnotizen.
- Pro Buch können frei benannte Angaben gespeichert werden, etwa Wissensstand, Wohnort, Beziehungsstatus, Besitz, Verletzungen oder andere Veränderungen.
- Ein Story-Bibel-Eintrag kann beliebig viele solcher Angaben pro Band besitzen.
- Die allgemeine Akte wird dabei nicht dupliziert.
- So kann dieselbe Figur über eine komplette Reihe hinweg gemeinsam gepflegt werden, während Veränderungen je Band separat dokumentiert werden.

### ✒️ Kontinuität beim Schreiben

- Verknüpfte Story-Bibel-Akten zeigen im Manuskript-Inspector jetzt den für das aktuelle Buch hinterlegten Kontinuitätsstand.
- Relevante offene Handlungsfäden einer verknüpften Akte werden ebenfalls direkt beim Schreiben sichtbar.
- Dadurch kann beim Schreiben einer Szene geprüft werden, was eine Figur in diesem Band bereits weiß oder welche offenen Fäden mit ihr verbunden sind.

### ⌂ Hinweise auf der Buchübersicht

- Die Buchübersicht zeigt jetzt dezent die Anzahl offener bzw. in Entwicklung befindlicher Handlungsfäden.
- Zusätzlich wird angezeigt, wie viele bandspezifische Kontinuitätsnotizen für das aktuelle Buch vorhanden sind.
- Von diesen Hinweisen kann direkt in die Handlungsfäden bzw. Story-Bibel gewechselt werden.

### 🔎 Globale Suche

- Handlungsfäden sind Bestandteil der globalen Suche.
- Durchsucht werden Titel, Beschreibung, Typ, Status, Verlaufsnotizen, verknüpfte Bücher und verknüpfte Story-Bibel-Akten.
- Suchtreffer führen direkt zum entsprechenden Handlungsfaden.

### 💾 Datenmodell & Backup

- IndexedDB wurde um einen eigenen Speicherbereich für Handlungsfäden erweitert.
- Das Datenbankschema wurde auf Version 6 erweitert.
- Das vollständige Backupformat wurde auf Version 6 erweitert.
- v0.40-Backups enthalten Handlungsfäden, deren Verlauf und die bandspezifischen Kontinuitätsstände der Story-Bibel.
- Ältere v5- und v4-Backups bleiben weiterhin importierbar.
- Alle Kontinuitäts- und Handlungsfadendaten bleiben lokal im Browser.

## v0.39.0

### 🧭 Durchgängiger Buch-Workflow

- Der Arbeitsweg innerhalb eines Buches wurde noch einmal als zusammenhängender Prozess überarbeitet.
- Neben Ideen, Planung, Manuskript, Anhang und Buchdetails besitzt jedes Buch jetzt eine eigene Übersicht.
- Die einzelnen Bereiche bleiben jederzeit frei erreichbar; der Workflow zwingt nicht zu einer starren linearen Reihenfolge.
- Bereits geschriebene Kapitel können parallel zu noch groben Ideen und unfertigen Planungsblöcken existieren.

### ⌂ Buchübersicht

- Bücher besitzen jetzt einen eigenen Bereich „Übersicht“.
- Die Übersicht zeigt den aktuellen Entwicklungsstand von Ideen über Planung und Schreiben bis zur Fertigstellung.
- Sichtbar sind:
  - Anzahl der buchbezogenen Ideen
  - Anzahl der Planungsblöcke
  - Anzahl der Kapitel und Szenen
  - aktueller Wortstand
  - zuletzt bearbeitete Manuskriptabschnitte
  - Anzahl der verknüpften Story-Bibeln
  - Anzahl direkter Buchverknüpfungen
- Über „Weiterschreiben“ bzw. „Weiterarbeiten“ gelangt man direkt an die sinnvollste nächste Stelle.
- Zuletzt bearbeitete Kapitel und Szenen können direkt aus der Übersicht geöffnet werden.

### 📌 Nächster Schritt

- Jedes Buch besitzt jetzt eine freie Notiz „Wo mache ich weiter?“.
- Dort kann beispielsweise notiert werden, welcher Dialog als Nächstes geschrieben, welche Kontinuitätsfrage geprüft oder welcher Handlungsfaden weitergeführt werden soll.
- Die Notiz wird automatisch lokal gespeichert.
- Sie ist zusätzlich über die globale Buchsuche auffindbar.

### 🧩 Planung wird feiner

- Die Buchplanung unterstützt jetzt neben Kapiteltrennern auch eigene Szenentrenner.
- Szenentrenner können frei auf der Planungswand eingefügt und per Drag & Drop verschoben werden.
- Vor einzelnen Ideen oder Textblöcken kann direkt ein neuer Kapitel- oder Szenenbeginn eingesetzt werden.
- Dadurch kann eine zunächst grobe Reihenfolge schrittweise in Kapitel und anschließend in Szenen verfeinert werden.

### 🔄 Planung → Kapitel → Szenen → Manuskript

- Die Übergabe der Buchplanung erzeugt jetzt eine echte Kapitel-/Szenenstruktur.
- Kapiteltrenner werden zu Kapitelobjekten.
- Darunter liegende Planungsbereiche werden als Szenen angelegt.
- Szenentrenner bestimmen zusätzliche Szenengrenzen.
- Auch eine noch sehr grobe Planung ohne manuell gesetzte Trenner kann weiterhin übergeben werden.
- Bereits übertragene Planungsinhalte werden nicht erneut dupliziert.
- Werden später neue Planungsblöcke ergänzt, können ausschließlich diese neuen Inhalte nachgereicht werden.
- Bereits geschriebener Manuskripttext wird dabei nicht verändert.

### 🚦 Status der Planungsblöcke

- Jeder Planungsblock zeigt jetzt, wo er sich im Schreibprozess befindet.
- Mögliche Anzeigen sind unter anderem:
  - eingeplant
  - im Manuskript
  - Rohfassung
  - überarbeiten
  - geschrieben
- Der Status wird aus den tatsächlich mit dem Planungsblock verbundenen Manuskriptabschnitten abgeleitet.
- Dadurch ist auf der Planungswand direkt sichtbar, welche Ideen bereits im Text angekommen sind.

### 📎 Story-Bibel direkt an Szenen

- Manuskriptabschnitte können jetzt direkt mit Einträgen aus verknüpften Story-Bibeln verbunden werden.
- Im Inspector steht dafür ein eigener Bereich „Story-Bibel“ zur Verfügung.
- Durchsucht werden die für das aktuelle Buch relevanten Figuren, Orte, Weltinformationen und sonstigen Story-Bibel-Einträge.
- Einträge können mit einem Klick an die aktuelle Szene bzw. das aktuelle Kapitel angehängt werden.
- Verknüpfte Akten lassen sich direkt aus dem Inspector öffnen.
- Verknüpfungen können wieder gelöst werden, ohne den Story-Bibel-Eintrag selbst zu verändern.
- Beim Zusammenführen zweier Manuskriptabschnitte werden ihre Story-Bibel-Verknüpfungen zusammengeführt.

### 📍 Schreibposition merken

- Der Schreibeditor merkt jetzt zusätzlich die Cursorposition innerhalb jedes Manuskriptabschnitts.
- Beim erneuten Öffnen eines Kapitels oder einer Szene versucht die Suite, an die zuletzt verwendete Textstelle zurückzukehren.
- Die Position wird lokal gespeichert und ergänzt die bereits vorhandene Erinnerung an den zuletzt geöffneten Manuskriptabschnitt.
- Dadurch kann nicht nur das richtige Kapitel, sondern auch die ungefähre Stelle innerhalb des Textes wiedergefunden werden.

### 🛡️ Sicherere Strukturänderungen

- Vor dem Verschieben eines Manuskriptabschnitts in der Kapitel-/Szenenstruktur wird jetzt automatisch ein Sicherheitsstand des betroffenen Abschnitts angelegt.
- Die bereits vorhandenen Sicherheitsstände vor Teilen, Zusammenführen, Löschen und Wiederherstellen bleiben bestehen.
- Dadurch sind nun auch Drag-&-Drop-Strukturänderungen besser abgesichert.

### 🔎 Buchweite Suche

- Zusätzlich zur Suche im aktuell geöffneten Abschnitt gibt es jetzt eine Suche über das gesamte Manuskript eines Buches.
- Treffer werden nach Kapitel bzw. Szene gruppiert angezeigt.
- Die Trefferanzeige zeigt einen Textausschnitt und die Anzahl der Fundstellen pro Manuskriptabschnitt.
- Von einem Treffer kann direkt in den entsprechenden Abschnitt gesprungen werden.

### 🔁 Buchweites Ersetzen

- Suchbegriffe können jetzt über das gesamte Manuskript eines Buches hinweg ersetzt werden.
- Vor einer solchen Massenänderung wird eine Bestätigung mit der Anzahl der betroffenen Treffer und Abschnitte angezeigt.
- Vor jedem tatsächlich veränderten Manuskriptabschnitt wird automatisch ein eigener Sicherheitsstand erzeugt.
- Erst danach erfolgt das Ersetzen.
- Wortstände von Buch und Romanprojekt werden anschließend neu berechnet.

### 📤 Manuskript-Export

- Das komplette Buchmanuskript kann jetzt als TXT exportiert werden.
- Zusätzlich steht ein HTML-Export mit grundlegender Kapitel- und Szenenstruktur zur Verfügung.
- Exportiert werden ausschließlich Manuskriptüberschriften und Manuskripttext.
- Interne Planungsdaten, Story-Bibel-Akten, Inspector-Notizen und andere Arbeitsinformationen werden nicht in das eigentliche Lesemanuskript geschrieben.
- Der Export verändert keine gespeicherten Daten.

### 🧱 Freier statt starrer Workflow

- Die Suite behandelt Ideen, Planung und Manuskript weiterhin nicht als abgeschlossene Pflichtphasen.
- Ein Buch kann bereits Manuskripttext enthalten, während andere Abschnitte noch als lose Idee existieren.
- Planungsblöcke dürfen nach der ersten Manuskriptübergabe weiterwachsen.
- Story-Bibel, Planung und Text bleiben parallel bearbeitbar.
- Damit unterstützt die Autoren-Suite ausdrücklich einen Schreibprozess, der vom Groben immer weiter ins Feine geht.

## v0.38.0

### 📎 Verknüpfbare Anhänge & Story-Bibeln

- Bücher können jetzt eigene Anhänge bzw. Story-Bibeln besitzen.
- Ein Anhang ist kein fest eingebauter Bestandteil nur eines Buches, sondern ein eigenständiger Wissensbereich.
- Derselbe Anhang kann mit beliebig vielen Büchern verknüpft werden.
- Dadurch können beispielsweise mehrere Bände einer Reihe dieselben Figuren-, Orts- und Weltinformationen gemeinsam verwenden.
- Ein Buch kann gleichzeitig mehrere unterschiedliche Anhänge besitzen, etwa eine gemeinsame Reihen-Bibel und einen zusätzlichen bandspezifischen Anhang.

### 📚 Mehrere Bücher pro Anhang

- Beim Anlegen oder Bearbeiten eines Anhangs können mehrere Bücher ausgewählt werden.
- Bestehende Anhänge können direkt aus einem anderen Buch heraus zusätzlich verknüpft werden.
- Eine Verknüpfung kann wieder von einem einzelnen Buch gelöst werden, ohne den Anhang oder seine Inhalte für die übrigen Bücher zu löschen.
- Wird ein Buch aus der Bibliothek entfernt, bleiben gemeinsam genutzte Anhänge erhalten; lediglich die Verknüpfung zum gelöschten Buch wird entfernt.

### 👤 Figurenakten

- Story-Bibeln können ausführliche Figurenakten enthalten.
- Figuren besitzen Titel/Name, Rolle bzw. Untertitel, Kurzbeschreibung, freie Notizen, Bilder und einen individuell konfigurierbaren Steckbrief.
- Für neue Figuren wird eine bearbeitbare Ausgangsvorlage angeboten, unter anderem mit Alter, Geburtstag, Größe, Aussehen, Augen, Haaren, Beruf/Rolle, Stärken, Schwächen, Ängsten, Wünschen und Geheimnissen.
- Sämtliche Felder können gelöscht, umbenannt oder durch eigene Felder ersetzt werden.
- Die Figurenakte ist dadurch nicht auf ein bestimmtes Genre oder Figurenmodell festgelegt.

### 🏞️ Orte, Weltwissen & weitere Eintragstypen

- Neben Figuren können folgende Arten von Story-Bibel-Einträgen angelegt werden:
  - Orte
  - Welt & Lore
  - Organisationen
  - Gegenstände
  - Tiere / Kreaturen
  - Regeln / Systeme
  - Ereignisse
  - freie Notizen
- Jeder Eintrag verwendet dieselbe flexible Grundstruktur.
- Für verschiedene Typen stehen passende, vollständig veränderbare Steckbriefvorlagen als Ausgangspunkt bereit.
- Damit können sowohl kleine Einzelnotizen als auch ausführliche Weltbau-Akten aufgebaut werden.

### 🧩 Frei anpassbare Steckbriefe

- Jeder Story-Bibel-Eintrag kann beliebig viele eigene Steckbrieffelder besitzen.
- Ein Feld besteht aus frei benennbarer Bezeichnung, Feldtyp und Inhalt.
- Unterstützte Feldtypen sind:
  - Text
  - Langtext
  - Zahl
  - Datum
  - Link
- Felder können jederzeit hinzugefügt oder wieder entfernt werden.
- Ein Wechsel des Eintragstyps kann bei neuen Einträgen eine passende neue Vorlage erzeugen.
- Bereits eingetragene Werte werden dabei nicht ohne Rückfrage verworfen.

### 🖼️ Bilder & Galerien

- Jeder Eintrag kann mehrere Bilder besitzen.
- Bilder werden vor dem lokalen Speichern automatisch verkleinert.
- Ein Bild kann als Hauptbild markiert werden.
- Jedes Bild kann eine eigene Bildnotiz erhalten.
- Dadurch lassen sich bei Figuren beispielsweise Portrait und weitere Referenzen, bei Orten Karten und Moodbilder oder bei Weltbau-Einträgen mehrere visuelle Referenzen gemeinsam speichern.
- Das Hauptbild wird direkt auf der Karte des Story-Bibel-Eintrags angezeigt.

### 🎯 Buchabhängige Einträge

- Ein Eintrag kann für alle Bücher seines Anhangs gelten.
- Alternativ kann festgelegt werden, dass eine Figur, ein Ort oder eine Weltinformation nur für einzelne verknüpfte Bände gilt.
- Beim Öffnen des Anhangs innerhalb eines Buches werden nur die für dieses Buch relevanten Einträge angezeigt.
- Dadurch kann eine gemeinsame Reihen-Bibel sowohl allgemeine Informationen als auch bandspezifische Inhalte enthalten, ohne Akten duplizieren zu müssen.

### 📖 Anhang direkt im Buch

- Jedes geöffnete Buch besitzt jetzt einen eigenen Workflow-Tab „📎 Anhang“.
- Dort werden alle mit dem Buch verknüpften Story-Bibeln angezeigt.
- Zwischen mehreren Anhängen kann direkt gewechselt werden.
- Neue Anhänge und Einträge können angelegt werden, ohne den Buchkontext zu verlassen.
- Der Buchdetailbereich zeigt jetzt zusätzlich, wie viele Anhänge mit dem Buch verbunden sind, und bietet einen direkten Sprung in die Story-Bibel.

### ✒️ Verbindung zum Schreibeditor

- Relevante Story-Bibel-Einträge werden während des Schreibens zusätzlich im Kontextbereich des aktuellen Buches angezeigt.
- Figuren, Orte und andere Wissenseinträge können dadurch als Referenz direkt beim Manuskript sichtbar bleiben.
- Ein Klick auf einen Story-Bibel-Eintrag öffnet dessen vollständige Akte.
- Vom Schreibeditor kann direkt in den Anhang des aktuellen Buches gewechselt werden.

### 🔎 Globale Suche

- Anhänge und Story-Bibel-Einträge sind jetzt Bestandteil der globalen Suche.
- Durchsucht werden unter anderem:
  - Titel und Namen
  - Untertitel / Rolle
  - Kurzbeschreibung
  - freie Notizen
  - sämtliche individuellen Steckbrieffelder
  - Werte der Steckbrieffelder
  - Bildnotizen
  - Name des Anhangs
  - verknüpfte Bücher
- Dadurch kann auch ein exakter Textbestandteil aus einer Charakter-, Orts- oder Weltnotiz wiedergefunden werden.
- Suchtreffer führen direkt zum passenden Anhang bzw. Eintrag.

### 💾 Datenmodell & Backup

- IndexedDB wurde um eigene Speicherbereiche für Anhänge und Story-Bibel-Einträge erweitert.
- Das Backupformat wurde auf Version 5 erweitert.
- Neue vollständige Backups enthalten Anhänge, flexible Steckbriefe, Bilder und Buchzuordnungen.
- Backups aus älteren v4-Versionen bleiben weiterhin importierbar, auch wenn sie noch keine Story-Bibel-Daten besitzen.
- Anhänge und ihre Einträge bleiben vollständig lokal und benötigen keinen Account oder externen Server.

### ✓ Systemcheck

- Der Systemcheck prüft jetzt zusätzlich Story-Bibel-Daten.
- Er erkennt Anhänge, die auf nicht mehr vorhandene Bücher verweisen.
- Story-Bibel-Einträge ohne vorhandenen Anhang werden als kritische Auffälligkeit erkannt.
- Einträge mit Verweisen auf nicht mehr vorhandene Bücher werden gemeldet.
- Auch bandspezifische Einträge, deren Buch nicht mehr zum übergeordneten Anhang gehört, werden sichtbar gemacht.
- Die Prüfung bleibt rein diagnostisch und verändert keine Story-Bibel-Inhalte automatisch.

## v0.37.0

### 🔗 Buchverknüpfungen

- Bücher können jetzt direkt miteinander verknüpft werden.
- Die Verknüpfung ist nicht auf Bücher desselben Romanprojekts oder derselben Reihe beschränkt.
- Dadurch können auch Spin-offs, Prequels oder Bücher aus einem gemeinsamen Universum über mehrere Projekte hinweg verbunden werden.
- Jede Verknüpfung kann eine freie Notiz erhalten, beispielsweise „spielt sechs Monate später“ oder „Nebenfigur wird hier Hauptfigur“.

### ↔️ Beziehungstypen

- Unterstützte Buchbeziehungen sind:
  - Vorgänger von
  - Nachfolger von
  - Prequel zu
  - Hat Prequel
  - Fortsetzung von
  - Hat Fortsetzung
  - Spin-off von
  - Ursprungsbuch eines Spin-offs
  - Parallelgeschichte zu
  - Gleiches Universum
  - Inhaltlich verbunden
- Beziehungen können jederzeit bearbeitet oder wieder gelöst werden.

### 🔄 Automatische Gegenverknüpfungen

- Gerichtete Beziehungen erzeugen automatisch die passende Gegenbeziehung im anderen Buch.
- Beispiel:
  - Band 3 ist „Vorgänger von“ Band 4.
  - Band 4 erhält automatisch „Nachfolger von“ Band 3.
- Prequel-, Fortsetzungs- und Spin-off-Beziehungen erzeugen ebenfalls passende Gegenverknüpfungen.
- Symmetrische Beziehungen wie „Parallelgeschichte“, „Gleiches Universum“ oder „Inhaltlich verbunden“ werden auf beiden Seiten gleich dargestellt.
- Beim Bearbeiten einer Beziehung wird die Gegenverknüpfung automatisch aktualisiert.
- Beim Löschen einer Beziehung wird auch die zugehörige Gegenverknüpfung entfernt.

### 📖 Buchdetailseite

- Die Buchdetailseite besitzt jetzt einen eigenen Bereich „Verbundene Bücher“.
- Verknüpfte Bücher werden mit Cover bzw. Buchrückenfarbe, Titel, Beziehungstyp und optionaler Verbindungsnotiz angezeigt.
- Das zugehörige Romanprojekt des anderen Buches wird ebenfalls sichtbar.
- Ein Klick auf ein verbundenes Buch öffnet direkt dessen Buchakte bzw. Arbeitsbereich.
- Verknüpfungen können direkt aus der Buchdetailseite angelegt und bearbeitet werden.
- Eine kleine Zusammenfassung zeigt, welche Beziehungstypen am aktuellen Buch vorhanden sind.

### 📚 Reihen & Folgebände

- „Nächsten Band“ erzeugt jetzt nicht nur ein neues Entwurfsbuch im selben Romanprojekt.
- Der neue Band wird zusätzlich automatisch als Nachfolger des bisherigen Buches verknüpft.
- Das bisherige Buch erhält entsprechend automatisch die Gegenbeziehung als Vorgänger.
- Dadurch entsteht beim Ausbau einer Reihe schrittweise ein echtes Buchnetz statt nur einer losen Bandnummerierung.

### 🌍 Projektübergreifende Verbindungen

- Bücher können mit Büchern aus anderen Romanprojekten verknüpft werden.
- Das ermöglicht gemeinsame Universen, Crossover, Spin-offs oder andere inhaltliche Beziehungen auch dann, wenn die Bücher unterschiedliche Projektwelten besitzen.
- Die Auswahl im Verknüpfungsdialog ist nach Romanprojekten gruppiert, damit auch große Bibliotheken übersichtlich bleiben.

### 🔎 Globale Suche

- Die globale Buchsuche berücksichtigt jetzt zusätzlich Titel verbundener Bücher.
- Auch Beziehungstypen und freie Verbindungsnotizen fließen in die Suche ein.
- Dadurch kann ein Buch beispielsweise über den Namen seines Spin-offs oder eine notierte Verbindung wiedergefunden werden.

### 🗑️ Löschen & Aufräumen

- Wird ein leeres Buch aus der Bibliothek gelöscht, werden direkte Buchverknüpfungen zu diesem Buch auch aus den übrigen Büchern entfernt.
- Dadurch bleiben keine absichtlich erzeugten toten Verbindungen zurück.
- Bücher mit Manuskript bleiben weiterhin gegen versehentliches Löschen geschützt.

### ✓ Systemcheck

- Der Systemcheck prüft jetzt zusätzlich das Buchnetz.
- Er erkennt Verknüpfungen zu nicht mehr vorhandenen Büchern.
- Fehlende automatische Gegenverknüpfungen werden sichtbar gemacht.
- Selbstverknüpfungen eines Buches werden als Auffälligkeit gemeldet.
- Der Systemcheck bleibt rein diagnostisch und repariert oder löscht keine Beziehungen automatisch.

### 💾 Datensicherheit

- Buchverknüpfungen werden direkt im jeweiligen Buchdatensatz gespeichert.
- Beziehungen und Gegenbeziehungen sind Bestandteil des regulären vollständigen Backups.
- Das Verknüpfen oder Lösen von Büchern verändert keine Manuskript-, Ideen-, Figuren- oder Weltdaten.
- Die bestehende Autosave-, Recovery-, Versions- und Backup-Architektur bleibt vollständig erhalten.

## v0.36.0

### 📚 Bücher in jedem Stadium

- Bücher können jetzt direkt in dem Stadium angelegt werden, in dem sie sich tatsächlich befinden.
- Verfügbare Einstiege sind:
  - Entwurfsbuch
  - direkt schreiben
  - Überarbeitung
  - fertig
  - veröffentlicht
- Der bisherige normale Status „Planung“ und „Pausiert“ bleiben zusätzlich verfügbar.
- Dadurch können zukünftige Buchideen, aktuelle Schreibprojekte und bereits abgeschlossene Werke gemeinsam im selben Bücherregal liegen.

### 🌱 Entwurfsbuch

- Entwurfsbücher öffnen weiterhin direkt den buchzentrierten Ideenbereich.
- Von dort führt der bestehende Workflow über Ideen → Planung → Manuskript.
- Ein Entwurfsbuch kann weiterhin bereits angelegt werden, bevor Plot, Kapitel oder vollständiges Romanprojekt ausgearbeitet sind.

### ✍️ Direkt schreiben

- Wird ein Buch als „Schreiben“ angelegt, öffnet sich unmittelbar sein Manuskript.
- Ideen- und Planungsbereich bleiben trotzdem jederzeit erreichbar.
- Damit muss ein Buch nicht zwingend zuerst durch die Planungsphasen laufen, wenn bereits direkt geschrieben werden soll.

### 📝 Überarbeitung

- Bücher im Status „Überarbeitung“ öffnen ebenfalls direkt im Manuskript.
- Der vollständige Schreibeditor, Inspector, Versionsverlauf, Autosave und Recovery bleiben verfügbar.
- Ideen, Planung und Romanprojekt bleiben parallel als Referenz erreichbar.

### 📕 Fertige Bücher

- Bereits fertige Bücher können direkt in die Bibliothek aufgenommen werden, auch wenn ihr Manuskript nicht innerhalb der Autoren-Suite geschrieben wurde.
- Fertige Bücher öffnen standardmäßig eine eigene Buchdetailseite statt direkt den Schreibeditor.
- Ein Cover kann direkt beim Anlegen hinterlegt werden.
- Eine endgültige Wortzahl und Seitenzahl können unabhängig von einem vorhandenen Manuskript gespeichert werden.
- Das Manuskript kann bei Bedarf trotzdem später geöffnet oder ergänzt werden.

### ✨ Veröffentlichte Bücher

- „Veröffentlicht“ ist jetzt ein eigener Buchstatus.
- Veröffentlichte Bücher erhalten eine eigene Buchdetailansicht.
- Folgende Publikationsdaten können hinterlegt werden:
  - Veröffentlichungsdatum
  - ISBN
  - Veröffentlichungsart
  - Verlag / Imprint
  - Seitenzahl
  - endgültige Wortzahl
  - Ausgabe / Format
  - Link zum Buch
  - freie Veröffentlichungsnotiz
- Unterstützte Veröffentlichungsarten sind Selfpublishing, Verlag, Klein-/Independent-Verlag und private Veröffentlichung.

### 🖼️ Cover im Bücherregal

- Fertige und veröffentlichte Bücher mit vorhandenem Cover können im Bücherregal mit sichtbarer Coverfront dargestellt werden.
- Dadurch heben sich bereits existierende Werke optisch von zukünftigen Entwurfs- und Schreibbänden ab.
- Bücher ohne Cover bleiben weiterhin als klassische Buchrücken sichtbar.
- Veröffentlichte Bücher erhalten zusätzlich eine dezente Veröffentlichungskennzeichnung.

### 📖 Buchdetailseite

- Jedes Buch besitzt jetzt zusätzlich zu Ideen, Planung und Manuskript einen Bereich „Buchdetails“.
- Die Detailseite zeigt Cover, Status, Reihe/Band, Untertitel und freie Buchnotizen.
- Publikationsdaten werden als eigene Buchakte dargestellt.
- Manuskriptwortzahl und endgültige Wortzahl können unabhängig voneinander sichtbar sein.
- Ideen, Planungsstand, Manuskriptumfang und das zugehörige Romanprojekt werden ebenfalls zusammengefasst.
- Von der Buchdetailseite kann direkt zu Ideen, Planung, Manuskript oder Romanprojekt gewechselt werden.

### 📚 Reihen & vorhandene Bände

- Die Buchdetailseite zeigt die weiteren Bücher derselben Reihe bzw. desselben Romanprojekts.
- Bereits veröffentlichte Bände, aktuelle Schreibbände und zukünftige Entwürfe können dadurch nebeneinander betrachtet werden.
- Reihe und Bandnummer bleiben die Sortiergrundlage im Bücherregal.
- Ein Klick auf einen anderen Band öffnet dessen eigene Buchakte bzw. Arbeitsumgebung.
- Über „Nächsten Band“ kann direkt ein neues Entwurfsbuch im selben Romanprojekt vorbereitet werden.
- Reihenname und nächste Bandnummer werden dabei automatisch als Ausgangspunkt übernommen.

### 🗂️ Bibliotheksfilter

- Das Bücherregal kann jetzt zusätzlich nach dem Entwicklungsstand gefiltert werden.
- Verfügbare Filter sind veröffentlicht, fertig, Überarbeitung, Schreiben, Planung, Entwurfsbuch und pausiert.
- Pro Romanprojekt zeigt das Regal eine kleine Zusammenfassung der vorhandenen Buchstadien.
- Suche und Projektfilter bleiben parallel nutzbar.
- Die Buchsuche berücksichtigt zusätzlich ISBN, Verlag, Ausgabe und Veröffentlichungsnotizen.

### 🔢 Wortzahlen

- Fertige und veröffentlichte Bücher können eine endgültige Wortzahl besitzen, auch wenn kein Manuskript in der Suite hinterlegt ist.
- Existiert bereits ein vollständiges Manuskript und wird ein Buch auf „Fertig“ oder „Veröffentlicht“ gesetzt, kann dessen aktueller Wortstand als Ausgangswert verwendet werden.
- Im Bücherregal wird bei abgeschlossenen Büchern bevorzugt die hinterlegte endgültige Wortzahl dargestellt.
- Manuskriptwortzahl und endgültige Wortzahl bleiben auf der Detailseite getrennt nachvollziehbar.

### 🔗 Romanprojekt & Reihenlogik

- Fertige, veröffentlichte und zukünftige Bücher können weiterhin zum selben Romanprojekt gehören.
- Dadurch teilen sich mehrere Bände weiterhin Figuren, Orte, Weltbau, Recherche, Inspiration und projektweite Daten.
- Das einzelne Buch behält gleichzeitig seinen eigenen Status, sein Cover, seine Buchdaten, seine Planung und sein Manuskript.
- Damit können beispielsweise drei veröffentlichte Bände und ein zukünftiges Entwurfsbuch derselben Reihe gemeinsam verwaltet werden.

### 🔎 Globale Suche

- Buchtreffer berücksichtigen jetzt zusätzlich ISBN, Verlag, Ausgabe und Veröffentlichungsnotizen.
- Bereits veröffentlichte Bücher können dadurch auch über ihre bibliografischen Daten wiedergefunden werden.

### 🛡️ Datensicherheit

- Publikationsdaten werden direkt im vorhandenen Buchdatensatz gespeichert.
- Sie sind Bestandteil des regulären vollständigen Backups.
- Ein veröffentlichtes oder fertiges Buch ohne Manuskript kann bewusst gelöscht werden, erhält davor aber eine deutlichere Warnung, dass Cover und Buchdaten entfernt werden.
- Bücher mit Manuskript bleiben weiterhin gegen versehentliches Löschen geschützt.
- Die bestehende Autosave-, Recovery-, Versions- und Backup-Architektur bleibt unverändert erhalten.

## v0.35.0

### 📚 Buchzentrierter Workflow

- Bücher sind jetzt nicht mehr nur der Endpunkt für fertige Planung, sondern können ganz am Anfang des Schreibprozesses stehen.
- Ein Buch kann als unfertiges „Entwurfsbuch“ angelegt werden, obwohl weder Plot noch Kapitelstruktur feststehen.
- Innerhalb eines geöffneten Buches gibt es jetzt einen durchgängigen Arbeitsweg:
  - Ideen
  - Planung
  - Manuskript
- Das Buch bleibt während aller drei Phasen mit seinem übergeordneten Romanprojekt verbunden.

### 🌱 Direkt mit einem Entwurfsbuch beginnen

- Ein neues Buch benötigt nicht mehr zwingend ein vorher manuell angelegtes Romanprojekt.
- Im Buchdialog steht die Option „Neues Romanprojekt automatisch aus diesem Buch“ zur Verfügung.
- Wird diese Option verwendet, legt die Suite im Hintergrund automatisch das passende Romanprojekt als gemeinsame Welt- und Planungsebene an.
- Das Buch selbst bleibt der sichtbare Arbeitscontainer.
- Der neue Status „Entwurfsbuch“ kennzeichnet Bücher, die sich noch ganz am Anfang befinden.

### 💡 Ideen direkt am Buch

- Jedes Buch besitzt jetzt eine eigene Liste verknüpfter Ideenzettel.
- Ideenzettel können direkt innerhalb des geöffneten Buches im gesamten Ideenarchiv gesucht werden.
- Die Suche berücksichtigt auch Textstücke aus längeren Notizen.
- Ein Klick verknüpft einen Zettel mit genau diesem Buch.
- Der ursprüngliche Zettel bleibt unverändert im zentralen Ideenarchiv erhalten.
- Buchbezogene Ideen werden zusätzlich in den projektweiten Ideenpool aufgenommen, damit sie auch in anderen verknüpften Bereichen wie dem Szenen-Inspector erreichbar bleiben.
- Eine Buchverknüpfung kann wieder gelöst werden, ohne den ursprünglichen Ideenzettel zu löschen.

### 🔗 Bestehende Projektideen übernehmen

- Bereits auf Romanprojekt-Ebene gesammelte Ideen können mit einem Klick in ein konkretes Buch übernommen werden.
- Dadurch müssen bestehende Projekte aus älteren Versionen nicht neu sortiert oder manuell nachgebaut werden.
- Die projektweite Zuordnung bleibt parallel bestehen.

### 🧩 Eigene Planungswand pro Buch

- Jedes Buch besitzt jetzt eine eigene Planungswand.
- Buchplanung und projektweite Planung bleiben bewusst getrennte Ebenen.
- Verknüpfte Ideenzettel können einzeln auf die Planungswand gelegt werden.
- Eigene freie Textblöcke können zwischen Ideen ergänzt werden.
- Kapiteltrenner können jederzeit eingefügt werden.
- Ideenzettel können innerhalb der Buchplanung um zusätzliche buchbezogene Planungstexte erweitert werden.
- Alle Planungsblöcke können per Drag & Drop frei sortiert werden.
- Über „Kapitel davor beginnen“ kann aus einer zunächst groben Abfolge später schrittweise eine Kapitelstruktur entstehen.

### 🔄 Projektplanung in ein Buch übernehmen

- Bereits vorhandene projektweite Planungswände können bewusst in ein konkretes Buch kopiert werden.
- Die kopierten Planungsblöcke erhalten eigene Buch-IDs und können danach unabhängig weiterentwickelt werden.
- Ideenzettel aus der übernommenen Projektplanung werden automatisch auch mit dem Buch verknüpft.
- Damit bleibt der bisherige Projekt-Workflow vollständig nutzbar und kann schrittweise in den neuen Buch-Workflow überführt werden.

### ✒️ Planung → Manuskript

- Die eigene Buchplanung kann direkt in das Manuskript desselben Buches übergeben werden.
- Kapiteltrenner bestimmen dabei die entstehenden Manuskriptabschnitte.
- Gibt es noch keine Kapiteltrenner, kann die grobe Planung zunächst als ein Entwurfsabschnitt übernommen werden.
- Bereits vorhandener Manuskripttext wird niemals überschrieben.
- Bei späteren Übergaben werden nur Planungsblöcke übernommen, die noch nicht im Manuskript angekommen sind.
- Dadurch kann die Planung weiter wachsen, während bereits an früheren Kapiteln geschrieben wird.
- Bereits übertragene Planungsblöcke werden auf der Planungswand mit „✓ im Manuskript“ markiert.

### ✍️ Manuskript

- Der dritte Schritt des Buch-Workflows öffnet den bereits vorhandenen vollständigen Schreibeditor.
- Kapitel, Szenen, Rich-Text-Editor, Inspector, Wortzählung, Autosave, Recovery, Versionsverlauf, Teilen und Zusammenführen bleiben erhalten.
- Ein leeres Manuskript bietet einen direkten Rücksprung zur Buchplanung.
- Beim Öffnen des Manuskript-Schritts wird der zuletzt verwendete Abschnitt des jeweiligen Buches wiederhergestellt, sofern vorhanden.

### 🔎 Szenen & Buchideen

- Der Szenen-Inspector berücksichtigt jetzt sowohl projektweite als auch buchbezogene Ideenzettel.
- Dadurch können Ideen zunächst nur an einem konkreten Band gesammelt und später direkt mit einzelnen Szenen verknüpft werden.
- Planungsverknüpfungen im Inspector erkennen sowohl projektweite als auch buchbezogene Planungsblöcke.

### 🗂️ Klarer Arbeitsfluss

- Direkt im geöffneten Buch stehen jetzt die Schritte „1 · Ideen“, „2 · Planung“ und „3 · Manuskript“ zur Verfügung.
- Der Wechsel zwischen den Phasen erfolgt ohne den Buchkontext zu verlassen.
- Das zugehörige Romanprojekt bleibt über einen eigenen Sprung erreichbar.
- Beim Verlassen des Manuskript-Schritts werden laufende Speicher- und Schreibsession-Vorgänge zuerst sauber abgeschlossen.

### 🔍 Suche

- Die globale Suche eines Buches berücksichtigt jetzt zusätzlich die Texte seiner verknüpften Ideenzettel.
- Dadurch kann ein Buch auch über eine darin gesammelte Idee wiedergefunden werden.

### ✓ Systemcheck

- Der Systemcheck prüft jetzt zusätzlich Buchverknüpfungen auf fehlende Ideenzettel.
- Auch Planungsblöcke eines Buches mit fehlenden Ideenzetteln werden erkannt.
- Die Prüfung bleibt rein diagnostisch und verändert keine Daten automatisch.

### 💾 Datensicherheit

- Buchbezogene Ideen und Planungswände werden direkt im vorhandenen Buchdatensatz gespeichert und sind Bestandteil des regulären Backups.
- Die ursprünglichen Ideenzettel werden beim Verknüpfen, Sortieren oder Entfernen aus einem Buch nicht verändert.
- Wiederholte Übergaben aus der Buchplanung überschreiben keine bereits geschriebenen Manuskriptabschnitte.
- Bücher mit vorhandenem Manuskript bleiben weiterhin gegen versehentliches Löschen geschützt.
- Die bestehende Autosave-, Notfallentwurf-, Versions- und Backup-Architektur bleibt vollständig erhalten.

## v0.34.0

### 🧪 Gründliche Stabilitätsrunde

- Die komplette Autoren-Suite wurde nach Einführung der neuen Buch-Ebene noch einmal statisch auf Laufzeitfehler, fehlende UI-Ziele, doppelte IDs, doppelte Funktionen und veraltete Manuskriptpfade geprüft.
- JavaScript-Syntax, permanente DOM-Referenzen und zentrale Navigationswege wurden erneut validiert.
- Mehrere konkrete Fehler und Inkonsistenzen aus der neuen Buchintegration wurden behoben.
- Diese Version ergänzt bewusst keine neue große Modulebene, sondern stabilisiert den Arbeitsfluss von Bibliothek, Romanprojekt und Schreibeditor.

### ✒️ Schreibeditor

- Ein fehlendes UI-Ziel für die Szenennotiz wurde ergänzt; dadurch kann das Öffnen eines Manuskriptabschnitts nicht mehr an der Szenennotiz-Darstellung scheitern.
- Beim Wechsel zwischen Kapiteln und Szenen wird jetzt auf das vollständige Speichern des vorherigen Abschnitts gewartet.
- Dadurch werden besonders schnelle Abschnittswechsel besser gegen konkurrierende Autosave-Vorgänge abgesichert.
- Das zuletzt geöffnete Kapitel bzw. die zuletzt geöffnete Szene wird pro Buch gespeichert.
- Beim erneuten Öffnen eines Buches landet man dadurch wieder im zuletzt verwendeten Manuskriptabschnitt.
- Gibt es noch keinen gespeicherten letzten Abschnitt, wird der zuletzt bearbeitete Manuskriptabschnitt als Ausgangspunkt verwendet.
- Die leere Schreibansicht spricht jetzt konsequent von Büchern statt von einem allgemeinen Romanprojekt.

### 🚑 Crash-Recovery

- Die Wiederherstellung eines lokalen Notfallentwurfs wurde korrigiert.
- Wiederhergestellte Inspector-Daten wie Kurzinhalt, POV, Ort, Zeit, Konflikt, Figuren und Szenennotiz werden jetzt zuerst in das aktive Dokumentmodell übernommen.
- Dadurch können diese Daten beim anschließenden Inspector-Aufbau nicht mehr versehentlich durch den älteren Datenbankstand überschrieben werden.
- Der Speicherstatus bleibt während einer wiederhergestellten Fassung korrekt auf dem laufenden Autosave-Zustand, statt vorschnell wieder „Gespeichert“ anzuzeigen.

### 📚 Bücherregal

- Das Bücherregal wurde optisch stärker wie ein echtes Regal aufgebaut.
- Bücher werden nach ihrem Romanprojekt gruppiert.
- Bei vielen Büchern entstehen mehrere eigene Regalreihen statt eines unübersichtlichen Umbruchs auf einer einzigen Regalfläche.
- Jedes Romanprojekt erhält im Regal einen kleinen Beschriftungsstreifen.
- Buchstatus, Wortstand, Reihe und Band können direkt am Buchrücken erkennbar sein.
- Kürzlich bearbeitete Bücher werden dezent hervorgehoben.
- Die Buchrückenbreite reagiert weiterhin auf den Manuskriptumfang.
- Die Schriftfarbe auf individuell gewählten Buchrückenfarben wird automatisch an helle bzw. dunkle Farben angepasst.

### 🖼️ Cover & Buchgestaltung

- Ein bereits gewähltes Cover kann jetzt wieder bewusst entfernt werden.
- Das Dateifeld für Coverbilder wird beim erneuten Öffnen des Buchdialogs sauber zurückgesetzt.
- Ein vorhandenes Cover bleibt weiterhin Bestandteil der Buchakte, ohne die konsistente Buchrücken-Darstellung im Regal aufzubrechen.

### 📖 Buch-Schreibarbeitsplatz

- Der aktuell geöffnete Band zeigt im Manuskript-Binder zusätzlich Reihe, Band, Untertitel und Status als Kontext.
- Das Bücherregal und die Buchbearbeitung bleiben direkt aus dem Schreibeditor erreichbar.
- Die Editor-Kopfzeile und Werkzeugleiste wurden für längere Schreibsitzungen stabiler angeheftet.
- Auf kleineren Fenstern fällt die Oberfläche weiterhin auf eine flexiblere Darstellung zurück.

### 🧩 Planungswand → Buch

- Bei Romanprojekten mit mehreren Büchern wird das Zielbuch jetzt niemals stillschweigend geraten.
- Stattdessen öffnet sich ein eigener Auswahl-Dialog.
- Erst nach einer bewussten Auswahl werden die Planungsabschnitte in das Manuskript des gewünschten Bandes übertragen.
- Bei genau einem vorhandenen Buch bleibt die direkte Übergabe erhalten.
- Gibt es noch kein Buch, kann weiterhin automatisch ein erstes Buch für die Planung angelegt werden.
- Vorhandener Manuskripttext wird bei der Übergabe weiterhin nicht überschrieben.

### 🕘 Timeline bei mehreren Büchern

- Die projektweite Timeline ist jetzt explizit buchbewusst.
- Verknüpfte Szenen zeigen zusätzlich, aus welchem Buch sie stammen.
- Die Position eines Manuskriptabschnitts wird innerhalb seines eigenen Buches berechnet.
- Die Auswahl eines Manuskriptabschnitts in einem Timeline-Ereignis ist nach Büchern gruppiert.
- Der Chronologie-Check vergleicht Manuskriptreihenfolgen jetzt innerhalb desselben Buches.
- Dadurch entstehen bei Romanprojekten mit mehreren Bänden keine falschen Rückblenden-Warnungen nur aufgrund des Bandwechsels.

### 📖 Romanprojekt-Dashboard

- Offene Punkte berücksichtigen jetzt die neue Buch-Ebene.
- Fehlt noch ein Buch, wird gezielt das Anlegen des ersten Bandes vorgeschlagen.
- Existiert bereits ein Buch ohne Manuskript, führt der Hinweis direkt in das Bücherregal bzw. den Bucharbeitsbereich.
- Zuletzt bearbeitete Manuskriptabschnitte zeigen jetzt auch ihren Buchtitel.
- Auch Schreibaktivitäten werden mit dem zugehörigen Buchkontext verständlicher dargestellt.

### 📦 Backup & Wiederherstellung

- Die vollständige Backup-Wiederherstellung wurde für relationale Buch-, Projekt- und Manuskriptdaten gehärtet.
- Bei einer vollständigen Wiederherstellung bleiben die ursprünglichen IDs erhalten.
- Dadurch bleiben Verknüpfungen zwischen Ideen, Romanprojekten, Büchern, Manuskriptabschnitten und weiteren Modulen intakt.
- Nach einer Wiederherstellung werden alle Suite-Daten vollständig neu aus IndexedDB geladen.
- Alte aktive Projekt-, Buch- und Dokumentzeiger werden dabei zurückgesetzt, damit sie nicht auf nicht mehr vorhandene Datensätze zeigen.
- Beim Zusammenführen eines Backups mit einem bestehenden Archiv werden verknüpfte Projekt-/Buchdaten nicht mehr mit neuen IDs importiert und dadurch unbemerkt beschädigt.
- Im Zusammenführen-Modus werden stattdessen nur Ideen, Kategorien und Register ergänzt; für eine vollständige Suite-Wiederherstellung wird ausdrücklich der Ersetzen-Modus verwendet.

### ✓ Systemcheck

- Doppelte Buch-IDs werden jetzt zusätzlich erkannt.
- Manuskriptabschnitte ohne Buchzuordnung werden als kritische Auffälligkeit gemeldet.
- Manuskriptabschnitte mit nicht vorhandenem Buch werden erkannt.
- Widersprüchliche Zuordnungen zwischen Buch und Romanprojekt werden erkannt.
- Auch auffällige Buch-/Projektzuordnungen von Schreibsessions werden sichtbar gemacht.
- Der Systemcheck bleibt rein diagnostisch und verändert keine Daten automatisch.

### 🛡️ Datensicherheit

- Bücher mit Manuskript bleiben weiterhin gegen versehentliches Löschen geschützt.
- Romanprojekte mit vorhandenen Büchern bleiben ebenfalls vor unbemerktem Mitlöschen geschützt.
- Teilen, Zusammenführen und Löschen von Manuskriptabschnitten aktualisieren weiterhin Buch- und Projektwortstände.
- Die bestehende mehrstufige Autosave-, Notfallentwurf- und Versionsarchitektur bleibt vollständig erhalten.

## v0.33.0

### 📚 Bücher & Bibliothek

- Die Autoren-Suite besitzt jetzt eine eigene Bibliothek für echte Buchobjekte.
- Bücher werden nicht als Dateien oder lose Dokumente behandelt, sondern als eigenständige Bestandteile der Schreibumgebung.
- Jedes Buch gehört zu einem Romanprojekt und besitzt ein eigenes Manuskript.
- Ein Romanprojekt kann beliebig viele Bücher bzw. Bände enthalten.
- Dadurch können Einzelromane, Reihen und größere gemeinsame Universen mit mehreren Bänden sauber getrennt organisiert werden.

### 📖 Digitales Bücherregal

- Der bisherige Hauptbereich „Schreiben“ wurde in der Hauptnavigation durch „Bücher“ ersetzt.
- Bücher erscheinen in einem visuellen Bücherregal mit echten Buchrücken.
- Titel werden direkt auf dem Buchrücken dargestellt.
- Reihe und Bandnummer können ebenfalls auf dem Rücken erscheinen.
- Die Dicke eines Buchrückens reagiert dezent auf den aktuellen Manuskriptumfang.
- Bücher mit eigenem Coverbild können alternativ als Coverkarte im Regal erscheinen.
- Ein Klick auf ein Buch öffnet direkt dessen eigenen Schreibarbeitsplatz.

### 🎨 Buchgestaltung

- Jedes Buch kann einen eigenen Titel und Untertitel erhalten.
- Reihenname und Bandnummer werden separat gespeichert.
- Für Bücher kann eine eigene Buchrückenfarbe gewählt werden.
- Ein eigenes Coverbild kann hochgeladen werden.
- Coverbilder werden wie andere lokale Bilddaten vor dem Speichern verkleinert.
- Status, Wortziel, Deadline und freie Buchnotizen werden direkt am Buch gepflegt.

### ✒️ Eigener Schreibeditor pro Buch

- Jedes Buch besitzt ein eigenes Manuskript mit Kapiteln und Szenen.
- Die bestehende Rich-Text-Schreibumgebung, Autosave-, Recovery- und Versionslogik bleibt vollständig erhalten.
- Kapitel und Szenen werden jetzt zusätzlich eindeutig einem Buch zugeordnet.
- Ein Buch zeigt ausschließlich seine eigenen Manuskriptabschnitte im Binder.
- Wortstand und Wortziel im Editor beziehen sich auf das aktuell geöffnete Buch.
- Zwischen mehreren Büchern desselben Romanprojekts kann direkt im Schreibbereich gewechselt werden.
- Über „Regal“ gelangt man jederzeit zurück zur Bibliothek.
- Über „Buch“ können Metadaten und Gestaltung des aktuell geöffneten Bandes bearbeitet werden.

### 🔗 Romanprojekt & Buch

- Die neue Struktur lautet jetzt:
  - Romanprojekt
  - Buch / Band
  - Kapitel
  - Szene
- Figuren, Orte, Beziehungsnetz, Weltbau, Recherche, Inspiration und projektweite Ideenzettel bleiben beim Romanprojekt angesiedelt.
- Das konkrete Manuskript liegt dagegen im jeweiligen Buch.
- Dadurch können mehrere Bände dieselben Figuren- und Weltdaten verwenden, ohne Manuskripte miteinander zu vermischen.
- Der Projektwortstand bleibt weiterhin als Summe aller zugehörigen Buchmanuskripte verfügbar.

### 🧩 Planung → Buch

- Die Planungswand übergibt neue Kapitel jetzt gezielt an ein Buch.
- Gibt es im Romanprojekt noch kein Buch, kann für die bestehende Planung automatisch ein erstes Buch angelegt werden.
- Existiert genau ein Buch, wird dieses direkt als Ziel verwendet.
- Bei mehreren Büchern wird keine beliebige Zuordnung geraten: Die Bibliothek wird geöffnet und das Zielbuch muss zuerst bewusst gewählt werden.
- Bereits vorhandener Manuskripttext wird bei der Übergabe weiterhin nicht überschrieben.

### 🔎 Suche & Navigation

- Bücher sind jetzt Bestandteil der globalen `Strg/Cmd + K`-Suche.
- Suchtreffer zeigen neben dem Buchtitel auch Reihe, Band und Romanprojekt.
- Manuskripttreffer öffnen jetzt direkt das Buch, zu dem die gefundene Szene oder das Kapitel gehört.
- Das Romanprojekt-Dashboard zeigt die Anzahl seiner Bücher und besitzt direkte Sprungpunkte ins Bücherregal bzw. zum Anlegen eines neuen Buches.

### 🔄 Migration vorhandener Manuskripte

- Bestehende Manuskripte aus v0.32.0 und älteren Versionen bleiben erhalten.
- Für vorhandene Projekt-Manuskripte ohne Buchzuordnung wird beim ersten Start automatisch ein passendes Buch angelegt.
- Der bisherige Romanprojekt-Titel wird dabei als Ausgangstitel des migrierten Buches verwendet.
- Zielwortzahl und Deadline des Projekts werden als Ausgangswerte übernommen.
- Bestehende Kapitel und Szenen werden diesem Buch zugeordnet, ohne ihren Inhalt zu verändern.
- Vorhandene Schreibsessions werden soweit möglich ebenfalls dem migrierten Buch zugeordnet.

### 🛡️ Schutz vor Datenverlust

- Ein Buch, das Manuskriptabschnitte enthält, kann nicht einfach gelöscht werden.
- Der Text muss zuerst bewusst bearbeitet bzw. entfernt werden; ein Buchlöschen vernichtet niemals automatisch Manuskripttext.
- Ein Buch mit Manuskript kann nicht unbemerkt in ein anderes Romanprojekt verschoben werden.
- Romanprojekte mit vorhandenen Büchern können ebenfalls nicht einfach gelöscht werden.
- Der Systemcheck erkennt Manuskriptabschnitte mit fehlender Buchzuordnung oder ungültigen Buchverknüpfungen.
- Bücher sind Bestandteil des regulären Backupformats.

### 💾 Datenmodell

- IndexedDB wurde um einen eigenen Speicherbereich für Bücher erweitert.
- Das Datenmodell bleibt lokal und benötigt weiterhin keinen Account oder Server.
- Dokumente besitzen zusätzlich eine `bookId`, während ihre Verbindung zum Romanprojekt bestehen bleibt.
- Die bestehende Sicherheitsarchitektur mit Notfallentwurf, Autosave und Versionsständen bleibt kompatibel.

## v0.32.0

### ✨ UI-/UX-Polish

- Die Autoren-Suite wurde visuell und in der Bedienung vereinheitlicht, ohne neue Datenlogik einzuführen.
- Buttons, Eingabefelder und Fokuszustände verhalten sich konsistenter über alle Module hinweg.
- Hover-, Aktiv- und Deaktiviert-Zustände wurden vereinheitlicht.
- Texte in Karten und Übersichten brechen robuster um und laufen seltener aus ihren Bereichen heraus.
- Leere Zustände wirken ruhiger und konsistenter.

### 🪟 Dialoge & Formulare

- Große Dialoge sind jetzt konsequenter auf die verfügbare Fensterhöhe begrenzt.
- Dialoginhalte können intern scrollen, ohne dass die gesamte Seite unkontrolliert springt.
- Aktionsleisten bleiben beim Scrollen langer Dialoge besser erreichbar.
- Formularfelder und Textbereiche besitzen einheitlichere Mindesthöhen und Abstände.
- Auf kleinen Fenstern werden mehrspaltige Formulare automatisch auf eine Spalte reduziert.

### ⌨️ Tastatur & Fokus

- Interaktive Elemente besitzen deutlichere `focus-visible`-Zustände.
- Die Bedienung per Tab-Taste wird dadurch nachvollziehbarer.
- Buttons und Eingabefelder erhalten konsistentere Fokusrahmen.
- Der Toast-/Statusbereich ist für unterstützende Technologien als Live-Status ausgezeichnet.
- Nutzer mit aktivierter Einstellung „Bewegung reduzieren“ erhalten stark reduzierte Animationen und Übergänge.

### 📚 Navigation bei vielen Bereichen

- Die Projekt-Tabs können bei schmalen Fenstern horizontal gescrollt werden, statt unkontrolliert umzubrechen.
- Auch die Hauptnavigation bleibt bei begrenzter Breite erreichbar.
- Große Dashboard- und Statistikbereiche passen sich auf kleineren Fenstern ruhiger an.
- Lange Titel, Notizen und Metadaten können besser umbrechen.

### ✒️ Schreibeditor

- Der Manuskripteditor besitzt robusteres Scrollverhalten und mehr Platzreserve für die Editorleiste.
- Fließtext bleibt auf eine angenehm lesbare Zeilenbreite begrenzt.
- Suchen-&-Ersetzen-Leiste und Inspector verhalten sich bei kleineren Fensterbreiten flexibler.
- Scrollbare Schreib- und Inspectorbereiche reservieren stabilen Platz für Scrollleisten, damit Layoutsprünge reduziert werden.

### ⚡ Start- & Laufzeitperformance

- Die automatische Integritätsprüfung beim Start blockiert den ersten sichtbaren Aufbau der Suite nicht mehr.
- Unterstützte Browser führen den Systemcheck bevorzugt in einer Leerlaufphase aus.
- Als Fallback wird die Prüfung leicht verzögert gestartet.
- Die bereits vorhandene verzögerte Archivsuche bleibt erhalten und verhindert unnötige Renderläufe bei schneller Eingabe.
- Bestehende Paginierung, Suchindex-Cache und modulbezogene Ansichten bleiben unverändert erhalten.

### 🛡️ Datensicherheit

- Der Systemcheck wird beim Öffnen des Bereichs „Datensicherheit“ frisch berechnet.
- Die UI-/UX-Runde verändert keine bestehenden Manuskript-, Ideen-, Projekt- oder Mediendaten.
- Es wurden keine automatischen Datenreparaturen eingeführt.
- Backupformat und bestehende lokale Speicherstruktur bleiben kompatibel.

## v0.31.0

### ⚙️ Performance & technische Härtung

- Diese Version konzentriert sich auf Stabilität, große Datenbestände und die langfristige Wartbarkeit der Autoren-Suite.
- Es wurden bewusst keine neuen großen Inhaltsmodule ergänzt.
- Besonders Suchfunktionen und Datenprüfungen wurden für umfangreiche Archive vorbereitet.

### ⚡ Globale Suche für große Archive

- Der Suchindex der globalen `Strg/Cmd + K`-Suche wird jetzt zwischengespeichert, statt bei jedem einzelnen Tastendruck vollständig neu aufgebaut zu werden.
- Normalisierte Suchwerte werden direkt im Suchindex vorbereitet und bei Folgeabfragen wiederverwendet.
- Die Suche wird leicht verzögert ausgelöst, damit schnelle Tastatureingaben nicht unnötig viele vollständige Suchläufe erzeugen.
- Änderungen an Ideen, Projekten, Manuskripten, Recherche, Inspiration oder Namenslisten invalidieren den Suchindex automatisch.
- Dadurch bleibt die Suche aktuell, ohne bei unveränderten Daten wiederholt dieselbe Vorarbeit zu leisten.
- Die bestehende Paginierung des Ideenarchivs bleibt erhalten und verhindert weiterhin, dass tausende Zettel gleichzeitig als DOM-Elemente gerendert werden.

### ✓ Systemcheck

- Der Bereich „Datensicherheit“ besitzt jetzt einen eigenen Systemcheck.
- Geprüft werden unter anderem doppelte Ideen-, Projekt- und Manuskript-IDs.
- Manuskriptabschnitte ohne vorhandenes Romanprojekt werden als kritische Auffälligkeit erkannt.
- Fehlende Kategorien, verwaiste Ideenverknüpfungen und ungültige Orts- oder Figurenbeziehungen werden als Hinweise sichtbar gemacht.
- Sicherheitsversionen gelöschter Manuskriptabschnitte werden erkannt, aber bewusst nicht als Fehler behandelt, da sie weiterhin als Rettungskopie dienen können.
- Der Systemcheck verändert oder löscht keine Daten.
- Beim Start wird zusätzlich automatisch eine stille Integritätsprüfung durchgeführt; kritische Auffälligkeiten werden gemeldet.

### 🌍 Schutz der Weltstruktur

- Beim Verschieben eines Ortes wird jetzt verhindert, dass ein Ort unter einen eigenen Unterort verschoben wird.
- Dadurch können keine neuen kreisförmigen Weltbau-Hierarchien angelegt werden.
- Der Systemcheck erkennt zusätzlich bereits vorhandene kreisförmige Ortshierarchien als Hinweis.

### 💾 Sicherere Datenbankfehler

- Schreibfehler in IndexedDB werden jetzt zentraler abgefangen und sichtbar gemeldet.
- Ein voller lokaler Browserspeicher wird ausdrücklich als solcher erkannt.
- Im Schreibeditor wird ein Speicherproblem zusätzlich direkt am Speicherstatus sichtbar.
- Unbehandelte Speicherfehler können dadurch nicht mehr so leicht unbemerkt bleiben.

### 📦 Backup-Prüfung

- Ein neu erzeugtes Backup wird vor dem Download noch einmal strukturell geprüft.
- Bei aktuellen Backupformaten müssen alle erwarteten Modulbereiche als gültige Datenlisten vorhanden sein.
- Doppelte Ideen-IDs innerhalb eines Backups werden erkannt.
- Ein Backup mit erkannten Strukturfehlern wird nicht als erfolgreich exportiert.
- Mehrfaches gleichzeitiges Starten eines Backup-Exports wird verhindert.
- Erfolgreiche Exporte werden ausdrücklich als geprüftes Backup bestätigt.

### 🔁 Datenänderungen & Suchindex

- Zentrale Schreib-, Aktualisierungs- und Löschoperationen invalidieren den globalen Suchindex automatisch.
- Dadurch erscheinen Änderungen nach dem nächsten Suchaufruf aktuell, ohne dauerhaft mehrere Suchkopien der Daten zu speichern.
- Manuskript-Autosave kann den Suchindex aktualisieren, ohne die eigentlichen Suchdaten permanent zu duplizieren.

### 🧱 Langfristige Stabilität

- Bestehende Datenmodelle und Backupformate bleiben kompatibel.
- Es wurden keine automatischen Reparaturen eingebaut, die im Zweifel Nutzerdaten verändern könnten.
- Auffälligkeiten werden zuerst diagnostiziert und sichtbar gemacht.
- Die technischen Änderungen dienen damit als Grundlage für den bevorstehenden großen Praxistest der Suite.

## v0.30.0

### 🛡️ Sicherheits- & Stabilitätsrunde

- Diese Version konzentriert sich bewusst auf den Schutz von Manuskripttexten und vorhandenen Projektdaten.
- Es wurden keine neuen großen Inhaltsmodule ergänzt.
- Die bestehende Autosave-, Recovery- und Versionslogik wurde zusammengeführt und verstärkt.

### 💾 Mehrstufiges Autosave

- Änderungen im Schreibeditor werden weiterhin sehr schnell automatisch gespeichert.
- Noch bevor der reguläre Datenbank-Speichervorgang läuft, wird der aktuelle Editorstand zusätzlich als lokaler Notfallentwurf geschützt.
- Während eines geöffneten Manuskriptabschnitts wird der Notfallentwurf regelmäßig erneuert.
- Beim Wechsel in einen anderen Browser-Tab oder beim Minimieren wird zusätzlich ein Sicherheits-Speichervorgang angestoßen.
- Beim Schließen oder Verlassen der Seite wird der aktuell sichtbare Text noch einmal in den lokalen Notfallschutz geschrieben.

### 🚑 Crash-Recovery

- Für jeden Manuskriptabschnitt kann ein unabhängiger lokaler Notfallentwurf bestehen.
- Wird nach einem unerwarteten Abbruch ein neuerer Notfallstand als die regulär gespeicherte Version gefunden, bietet die Suite dessen Wiederherstellung an.
- Ein Notfallentwurf wird erst entfernt, nachdem der reguläre Speichervorgang erfolgreich abgeschlossen wurde.
- Dadurch bleibt der kurzfristige Schutz von der normalen IndexedDB-Speicherung getrennt.

### ↶ Versionsverlauf

- Der Schreibbereich besitzt jetzt einen direkt erreichbaren Button „Versionen“.
- Automatische Sicherheitsstände eines Manuskriptabschnitts können dort eingesehen werden.
- Pro Manuskriptabschnitt werden jetzt bis zu 50 ältere Sicherheitsstände aufbewahrt.
- Die Versionen zeigen Zeitpunkt, Grund der Sicherung und Wortstand.
- Rich-Text-Inhalte behalten beim Versionsstand ihr Inhaltsformat.

### ♻️ Sichere Wiederherstellung

- Ältere Manuskriptversionen können direkt aus dem Versionsverlauf wiederhergestellt werden.
- Vor jeder Wiederherstellung wird automatisch noch einmal der aktuelle Manuskriptstand gespeichert.
- Erst danach wird die ausgewählte ältere Version eingesetzt.
- Dadurch kann eine versehentliche Wiederherstellung selbst wieder rückgängig gemacht werden.
- Der Projektwortstand wird nach einer Wiederherstellung automatisch neu berechnet.

### 🗑️ Schutz vor Löschen

- Die bereits vorhandene Sicherheitskopie vor dem Löschen eines Manuskriptabschnitts bleibt erhalten.
- Gelöschte Manuskripttexte besitzen dadurch weiterhin einen Versionsstand, auch wenn der eigentliche Abschnitt entfernt wurde.
- Kapitelunterstrukturen werden beim Löschen weiterhin nicht unbemerkt mitgelöscht.

### 🔒 Schutz vor Überschreiben

- Periodische Versionsstände werden jetzt zuverlässig abgeschlossen, bevor ein neuer Manuskriptstand den vorherigen überschreibt.
- Ein Versionsstand enthält neben Text, Titel und Notiz nun auch das gespeicherte Inhaltsformat.
- Das reduziert das Risiko, dass bei schnell aufeinanderfolgenden Speichervorgängen ein vorgesehener Sicherheitsstand noch nicht vollständig angelegt wurde.

### 🧱 Lokale Architektur

- Manuskript, Versionsstände und Notfallentwürfe bleiben vollständig lokal.
- Notfallentwürfe und reguläre Manuskriptdaten liegen bewusst in unterschiedlichen Browser-Speichermechanismen.
- Die Sicherheitsfunktionen benötigen weiterhin keinen Account, Server oder externen Cloud-Dienst.
- Bestehende Backup- und Exportfunktionen bleiben erhalten.

## v0.29.0

### 📊 Erweiterte Schreibstatistiken

- Der Statistikbereich wurde zu einer ausführlichen Schreibauswertung ausgebaut.
- Bestehende Filter nach Romanprojekt und Zeitraum gelten jetzt auch für die neuen Detailauswertungen.
- Automatische Schreibsessions und manuelle Wortzahl-Einträge bleiben weiterhin getrennt erfassbar und werden gemeinsam sinnvoll ausgewertet.

### 🏆 Persönliche Rekorde

- Die Statistik zeigt jetzt den bisher besten Schreibtag.
- Die wortstärkste einzelne Schreibsession wird als eigener Rekord geführt.
- Die längste Schreibsession wird anhand der tatsächlich gespeicherten Sessiondauer ermittelt.
- Die aktuelle Schreibserie bleibt direkt bei den persönlichen Bestwerten sichtbar.

### 📅 Produktive Wochentage

- Schreibaktivität wird nach Wochentagen ausgewertet.
- Montag bis Sonntag werden als vergleichbare Balken dargestellt.
- Dadurch wird sichtbar, an welchen Tagen im gewählten Zeitraum besonders viel geschrieben wurde.
- Die Auswertung basiert auf den tatsächlich erfassten Wortzahlen und nicht nur auf der Anzahl der geöffneten Sessions.

### 🕰️ Produktive Uhrzeiten

- Automatisch erfasste Schreibsessions werden nach Tageszeit ausgewertet.
- Die Tageszeiten sind in vier übersichtliche Zeitfenster gegliedert.
- So lässt sich erkennen, ob der eigene Schreibrhythmus eher morgens, mittags, abends oder nachts liegt.
- Manuelle historische Wordcounts werden bewusst nicht künstlich einer Uhrzeit zugeordnet.

### ⏱️ Session-Auswertung

- Für den gewählten Zeitraum werden durchschnittliche Wörter pro Session berechnet.
- Die durchschnittliche Sessiondauer wird angezeigt.
- Aus Wortzahl und tatsächlicher Schreibzeit wird ein durchschnittliches Schreibtempo in Wörtern pro Stunde berechnet.
- Die Anzahl der Sessions im aktuellen Filterzeitraum bleibt direkt vergleichbar.

### 🟩 365-Tage-Heatmap

- Eine neue Schreib-Heatmap zeigt die vergangenen 365 Tage auf einen Blick.
- Tage mit Schreibaktivität werden abhängig von ihrer Wortmenge unterschiedlich stark hervorgehoben.
- Beim Überfahren eines Tages werden Datum und Wortzahl angezeigt.
- Die Heatmap reagiert auf den gewählten Projektfilter und kann dadurch auch den Schreibverlauf eines einzelnen Romans zeigen.

### 🔎 Bestehende Statistiken

- Tages-, 7-Tage- und Zeitraum-Wortzahlen bleiben erhalten.
- Schreibzeit, aktuelle Serie, durchschnittlicher Schreibtag und bester Tag bleiben Bestandteil der Übersicht.
- Projektvergleich, Aktivitätsverlauf, Monatskalender und die Liste letzter Schreibaktivitäten bleiben vollständig erhalten.
- Die neuen Auswertungen ergänzen den bestehenden Statistikbereich statt ihn zu ersetzen.

### 💾 Datensicherheit

- Alle neuen Statistiken werden ausschließlich aus bereits vorhandenen Schreibdaten berechnet.
- Rekorde, Heatmap und Rhythmus-Auswertungen erzeugen keine zusätzlichen Kopien des Manuskripts.
- Es werden keine Schreibdaten verändert, wenn Filter oder Statistikzeiträume gewechselt werden.
- Bestehende Backups bleiben vollständig kompatibel.

## v0.28.0

### 🎯 Schreibziele & Challenges

- Romanprojekte können jetzt neben der Zielwortzahl auch eine Deadline erhalten.
- Ein eigenes Tagesziel kann unabhängig vom Gesamtziel festgelegt werden.
- Optional kann ein Challenge-Startdatum gespeichert werden.
- Damit lassen sich klassische Monats-Challenges ebenso abbilden wie individuelle Schreibphasen für einen Roman.
- Die Einstellungen befinden sich direkt in den bestehenden Projekteinstellungen.

### 📈 Dynamischer Schreibplan

- Das Projekt-Dashboard berechnet automatisch die noch fehlenden Wörter bis zum Gesamtziel.
- Bei gesetzter Deadline wird angezeigt, wie viele Wörter pro verbleibendem Tag aktuell nötig sind.
- Bei vorhandenem Challenge-Start erkennt die Suite zusätzlich, ob der aktuelle Wortstand vor oder hinter einem gleichmäßig verteilten Schreibplan liegt.
- Sobald das Gesamtziel erreicht ist, wird die Challenge entsprechend als erreicht dargestellt.
- Die Berechnung passt sich automatisch an den aktuellen Manuskriptstand an.

### ✍️ Tagesziel

- Das aktuelle Tagesziel wird direkt im Projekt-Dashboard angezeigt.
- Ein Fortschrittsbalken zeigt, wie viel des heutigen Ziels bereits geschafft wurde.
- Tatsächlich geschriebene Wörter und das Tagesziel bleiben getrennt vom Gesamtwortziel sichtbar.
- Tagesziele können jederzeit geändert werden, ohne vorhandene Schreibdaten zu verändern.

### 🔢 Manuelle Wortzahlen

- Wortzahlen können jetzt direkt im Romanprojekt schnell und unkompliziert manuell eingetragen werden.
- Zu jedem Eintrag wird ein Datum gewählt.
- Positive Werte können für neu geschriebene Wörter und negative Werte für Kürzungen verwendet werden.
- Dadurch können auch Schreibarbeit außerhalb des integrierten Editors und ältere Wordcount-Daten erfasst werden.
- Manuelle Einträge werden im bestehenden separaten Wordcount-Speicher abgelegt und verändern den Manuskripttext nicht.

### 🗓️ Schreibkalender

- Das Projekt-Dashboard zeigt einen kompakten Kalender für den aktuellen Monat.
- Tage mit erfasster Schreibaktivität werden hervorgehoben.
- Die jeweilige Wortzahl wird direkt im Kalendertag angezeigt.
- Wurde das festgelegte Tagesziel erreicht, erhält der Tag eine zusätzliche Markierung.
- Für die Challenge kann festgelegt werden, ob manuell eingetragene Wortzahlen in den Fortschritt einbezogen werden.

### 💾 Datensicherheit

- Schreibziele werden als Projekteinstellungen gespeichert und verändern keine Manuskriptinhalte.
- Manuelle Wordcounts bleiben getrennt von den eigentlichen Manuskriptständen.
- Änderungen an Zielwortzahl, Deadline oder Tagesziel löschen keine vorhandenen Statistikdaten.
- Bestehende Projekte ohne Schreibziele bleiben vollständig kompatibel und erhalten neutrale Standardwerte.
- Alle neuen Ziel- und Wordcount-Daten sind Bestandteil der bestehenden lokalen Datenspeicherung und Backups.

## v0.27.0

### 📖 Romanprojekt-Dashboard

- Die bisherige schlichte Projektübersicht wurde zu einer echten Roman-Zentrale ausgebaut.
- Beim Öffnen eines Romanprojekts ist jetzt sofort sichtbar, wie weit Planung, Manuskript, Figuren, Orte und Verknüpfungen gewachsen sind.
- Prämisse, Stimmung und Herkunft aus einer Romanidee bleiben direkt im Dashboard sichtbar.
- Der aktuelle Manuskriptwortstand wird automatisch aus den vorhandenen Kapiteln und Szenen berechnet.
- Ein vorhandenes Wortziel wird als große Fortschrittsanzeige dargestellt.
- Bereits heute geschriebene Wörter werden direkt beim Projektfortschritt angezeigt.

### ⚡ Schnellzugriffe

- Vom Dashboard aus kann direkt weitergeschrieben werden.
- Planungswand, Figurenakten und Timeline sind mit einem Klick erreichbar.
- Projekteinstellungen können unmittelbar geöffnet werden.
- Recherche und Inspiration des aktuellen Projekts lassen sich direkt aus den Kennzahlen öffnen.
- Zuletzt bearbeitete Manuskriptabschnitte führen direkt zurück in den Schreibeditor.

### 📊 Projekt auf einen Blick

- Zentrale Kennzahlen zeigen Planungsblöcke, Figuren, Orte, Ideenzettel, Recherche und Inspiration.
- Figuren werden als kleine visuelle Cast-Übersicht mit Portrait und Rolle dargestellt.
- Die zuletzt bearbeiteten Manuskriptabschnitte zeigen Typ, Wortzahl und aktuellen Arbeitsstatus.
- Letzte Schreibaktivitäten zeigen Wortveränderung, Datum, betroffenen Abschnitt und – sofern vorhanden – die Schreibdauer.

### 🧭 Offene Punkte

- Das Dashboard erkennt einige offensichtliche noch offene Bereiche eines Romanprojekts.
- Hinweise erscheinen beispielsweise bei fehlender Prämisse, leerer Planungswand, fehlenden Figuren oder Orten und noch nicht begonnenem Manuskript.
- Manuskriptabschnitte ohne Kurzinhalt oder noch im Status „Idee/Geplant“ werden ebenfalls sichtbar gemacht.
- Bei vorhandenen Manuskriptabschnitten ohne Timeline wird der Aufbau der Chronologie vorgeschlagen.
- Die Hinweise sind direkte Sprungpunkte in den jeweiligen Arbeitsbereich.

### 🕘 Timeline & Projektverbindungen

- Die ersten Timeline-Ereignisse werden direkt im Dashboard als Vorschau angezeigt.
- Timeline-Einträge können aus der Vorschau unmittelbar geöffnet werden.
- Eine eigene Projekt-Netz-Karte zeigt Figurenbeziehungen, Ortsverknüpfungen und Szenenlinks zu Ideen, Recherche und Inspiration.

### 🧭 Navigation

- Die Projekt-Tabs „Beziehungsnetz“, „Weltbau“ und „Timeline“ sind wieder vollständig in die zentrale Projekt-Navigation eingebunden.
- Alle Dashboard-Sprungpunkte verwenden dieselben bestehenden Projektbereiche und erzeugen keine doppelten Daten.

### 💾 Datensicherheit

- Das Romanprojekt-Dashboard ist vollständig lesend und wertet nur bereits vorhandene Projektdaten aus.
- Schnellzugriffe verändern keine Manuskript- oder Planungsdaten.
- Wortstand, Aktivitäten und Verknüpfungen werden aus den bestehenden lokalen Daten berechnet.
- Die Daten- und Backup-Struktur bleibt mit v0.26.0 kompatibel.

## v0.26.0

### 🔎 Globale Suche

- Die Autoren-Suite besitzt jetzt eine zentrale Suche über alle wichtigen Inhaltsbereiche.
- Durchsucht werden Ideenzettel, Romanprojekte, Figurenakten, Ortsakten, Manuskriptabschnitte, Recherche, Inspiration und Namenslisten.
- Die Suche berücksichtigt nicht nur Titel, sondern auch wichtige Inhalte wie Notizen, Beschreibungen, Tags, Figurenentwicklung, Recherchetexte und Manuskripttext.
- Treffer werden nach Inhaltsart gruppiert und nach Relevanz sortiert.
- Exakte Titelübereinstimmungen und Treffer am Titelanfang werden stärker gewichtet als Treffer tief im Inhalt.
- Auch längere Textstücke aus einer Notiz oder einem Manuskript können als Suchbegriff verwendet werden.

### ⌨️ Command Palette

- Mit `Strg + K` bzw. `Cmd + K` öffnet sich jetzt von überall die zentrale Command Palette.
- Die Palette kann alternativ über den neuen Button „Alles“ in der oberen Leiste geöffnet werden.
- Ohne Suchbegriff zeigt sie wichtige Schnellaktionen.
- Von dort lassen sich unter anderem Ideenarchiv, Romanprojekte, Schreibprogramm, Recherche und Namenslisten direkt öffnen.

### ↗️ Direkte Navigation

- Suchtreffer sind direkt anklickbar.
- Ideentreffer öffnen unmittelbar den betreffenden Zettel.
- Figuren- und Ortstreffer öffnen die passende Akte im zugehörigen Romanprojekt.
- Manuskripttreffer führen direkt in den betreffenden Abschnitt des Schreibprogramms.
- Recherche-, Inspirations- und Namenseinträge öffnen direkt ihren jeweiligen Bearbeitungsbereich.
- Romanprojekte können unmittelbar aus der Suche geöffnet werden.

### ⚡ Suche für große Archive

- Die globale Suche arbeitet auf einem kompakten, zur Laufzeit erzeugten Suchindex.
- Ergebnisse werden auf die relevantesten Treffer begrenzt, damit die Oberfläche auch bei sehr großen Archiven übersichtlich bleibt.
- Mehrteilige Suchbegriffe können auch dann Treffer finden, wenn die einzelnen Wörter an unterschiedlichen Stellen eines Datensatzes vorkommen.
- Die bestehende Spezialsuche des Ideenarchivs bleibt unverändert erhalten und kann weiterhin für besonders genaue Archivabfragen genutzt werden.

### 💾 Datensicherheit

- Die globale Suche ist vollständig lesend und verändert keine Inhalte.
- Es werden keine zusätzlichen Kopien der Manuskript- oder Archivdaten dauerhaft gespeichert.
- Die Command Palette greift direkt auf die bereits lokal gespeicherten Daten der Autoren-Suite zu.
- Bestehende Backups und Datenstrukturen bleiben unverändert kompatibel.

## v0.25.0

### 🕘 Timeline & Chronologie

- Romanprojekte besitzen jetzt einen eigenen Bereich „Timeline“.
- Ereignisse können unabhängig von der Manuskriptreihenfolge chronologisch angeordnet werden.
- Dadurch lassen sich Vorgeschichte, Haupthandlung, Rückblenden und parallele Ereignisse gemeinsam überblicken.
- Ereignisse können per Drag & Drop innerhalb der Timeline neu sortiert werden.

### 📅 Zeitliche Einordnung

- Timeline-Ereignisse können ein konkretes Datum erhalten.
- Zusätzlich steht eine freie Zeitangabe für fiktionale oder relative Zeit zur Verfügung.
- Beispiele sind „Tag 3“, „drei Jahre zuvor“, „in derselben Nacht“ oder „zwei Wochen später“.
- Eine eigene chronologische Positionsnummer erlaubt auch dann eine klare Reihenfolge, wenn keine exakten Daten verwendet werden.

### ✒️ Verbindung mit dem Manuskript

- Timeline-Ereignisse können direkt mit einem Kapitel oder einer Szene verknüpft werden.
- Die Position des verknüpften Abschnitts im Manuskript wird direkt auf der Timeline angezeigt.
- Szenen mit Zeitangaben oder Kurzinhalt können automatisch aus dem Schreibeditor in die Timeline übernommen werden.
- Bereits automatisch übernommene Szenen werden beim erneuten Synchronisieren aktualisiert statt dupliziert.
- Ort und auftretende Figuren aus dem Szenen-Inspector werden bei der Übernahme mitgenommen.

### 🎭 Figuren & Orte

- Timeline-Ereignisse können mit beliebig vielen Figuren verknüpft werden.
- Ein Ort aus den bestehenden Ortsakten kann direkt zugeordnet werden.
- Dadurch lassen sich Handlungsstränge, Reisen und parallele Figurenverläufe zeitlich besser nachvollziehen.

### 🧭 Chronologie-Check

- Die Timeline besitzt einen eigenen Chronologie-Überblick.
- Ereignisse ohne zeitliche Einordnung werden sichtbar gemacht.
- Die Anwendung erkennt mögliche Rückblenden, wenn chronologische Reihenfolge und Manuskriptreihenfolge voneinander abweichen.
- Mehrere Ereignisse am selben Datum werden als möglicher Hinweis auf parallele Handlungen ausgewertet.
- Die Prüfung verändert keine Daten, sondern dient ausschließlich als Orientierung.

### 💾 Datensicherheit

- Timeline-Ereignisse werden direkt im jeweiligen Romanprojekt gespeichert.
- Die Synchronisierung mit Manuskriptszenen überschreibt keine Manuskripttexte.
- Manuell ergänzte Timeline-Notizen bleiben beim erneuten Szenenabgleich erhalten.
- Alle Timeline-Daten und Verknüpfungen sind automatisch Bestandteil der Autoren-Suite-Backups.

## v0.24.0

### 🕸️ Beziehungsnetz

- Romanprojekte besitzen jetzt einen eigenen Bereich „Beziehungsnetz“.
- Figuren können direkt miteinander verbunden werden.
- Unterstützte Beziehungsarten sind Familie, Freundschaft, Romantik, Bündnis, Konflikt, Macht/Abhängigkeit, geheime Verbindung und freie Beziehungen.
- Jede Beziehung kann eine eigene Kurzbezeichnung und ausführliche Dynamiknotiz erhalten.
- Das Beziehungsnetz stellt die Figuren als visuelle Knoten und ihre Beziehungen als Verbindungslinien dar.
- Unterschiedliche Beziehungstypen werden in der Darstellung visuell voneinander unterschieden.
- Alle Beziehungen können nachträglich bearbeitet oder wieder entfernt werden.

### 🎭 Sichere Figurenbeziehungen

- Beziehungen werden direkt im jeweiligen Romanprojekt gespeichert.
- Wird eine Figur gelöscht, werden ausschließlich ihre Beziehungsverknüpfungen entfernt.
- Andere Figurenakten und deren Inhalte bleiben dabei unverändert.
- Das bisherige freie Beziehungsfeld in der Figurenakte bleibt zusätzlich erhalten und kann weiterhin für ausführlichere Notizen genutzt werden.

### 🌍 Weltbau

- Romanprojekte besitzen jetzt einen eigenen Bereich „Weltbau“.
- Ortsakten können hierarchisch miteinander verbunden werden.
- Beispiele für mögliche Strukturen sind Welt → Land → Region → Stadt → Gebäude oder Königreich → Provinz → Dorf.
- Jeder Ort kann einen übergeordneten Ort erhalten.
- Die vorhandenen Ortsakten mit Bildern, Atmosphäre, Beschreibung und Hintergrundinformationen bleiben die Grundlage des Weltbaus.

### 🌳 Ortshierarchie

- Die Weltbauansicht stellt Ortsstrukturen als eingerückten Baum dar.
- Verschachtelungsebenen werden visuell unterschieden.
- Direkt aus der Weltbauansicht können neue Hauptorte oder Unterorte angelegt werden.
- Unterorte übernehmen beim Anlegen automatisch den gewählten übergeordneten Ort als Ausgangspunkt.
- Vorhandene Orte können jederzeit in der Hierarchie neu zugeordnet werden.

### 🧭 Weltübersicht

- Die Weltbauansicht zeigt die Anzahl aller Orte, oberster Ebenen und die aktuelle maximale Verschachtelung.
- Bilder sowie Art und Region eines Ortes bleiben direkt in der Baumansicht sichtbar.
- Ein Klick auf einen Ort öffnet unmittelbar die zugehörige Ortsakte.

### 💾 Datensicherheit

- Beziehungsnetz und Weltbau werden innerhalb der bestehenden Romanprojektdaten gespeichert.
- Bestehende Figuren- und Ortsakten bleiben mit älteren Projekten kompatibel.
- Beim Löschen eines übergeordneten Ortes werden dessen direkte Unterorte nicht gelöscht, sondern sicher auf die oberste Ebene verschoben.
- Alle neuen Beziehungen und Hierarchien sind automatisch Bestandteil der Autoren-Suite-Backups.

## v0.23.0

### 🎭 Figurenakten

- Figuren im Romanprojekt wurden von einfachen Planungsnotizen zu ausführlichen Figurenakten erweitert.
- Jede Figur kann jetzt ein eigenes Portraitbild erhalten.
- Alter bzw. Lebensphase, Pronomen, Rolle im Roman und Beruf/Funktion können separat gepflegt werden.
- Aussehen und Persönlichkeit besitzen eigene ausführliche Felder.
- Motivation, Angst bzw. Schwäche und ein mögliches Geheimnis können strukturiert festgehalten werden.
- Die geplante Figurenentwicklung erhält mit „Entwicklung / Arc“ einen eigenen Bereich.
- Beziehungen und Dynamiken zu anderen Figuren können frei beschrieben werden.
- Die bereits vorhandenen Verknüpfungen zu Namenslisten, Ideenzetteln, Recherche und Inspiration bleiben vollständig erhalten.

### 🖼️ Figurenbilder

- Portraitbilder können direkt in der Figurenakte hochgeladen werden.
- Bilder werden vor der lokalen Speicherung automatisch verkleinert.
- Figuren werden in der Projektübersicht jetzt mit Portrait und wichtigen Akteninformationen dargestellt.
- Fehlt ein Bild, bleibt eine neutrale visuelle Platzhalterdarstellung erhalten.

### ⌖ Ortsakten

- Orte wurden ebenfalls zu ausführlicheren Ortsakten erweitert.
- Jeder Ort kann ein eigenes Referenzbild erhalten.
- Art des Ortes und Region bzw. Welt werden separat erfasst.
- Atmosphäre und ausführliche Beschreibung besitzen eigene Felder.
- Die Bedeutung eines Ortes für die Geschichte kann unabhängig von seiner allgemeinen Beschreibung notiert werden.
- Sinneseindrücke wie Geräusche, Gerüche, Licht oder Temperatur können gesammelt werden.
- Geschichte, Regeln und besondere Eigenschaften eines Ortes erhalten einen eigenen freien Bereich.

### 🗂️ Projektübersicht

- Die bisherigen Tabs „Figuren“ und „Orte“ heißen jetzt „Figurenakten“ und „Ortsakten“.
- Figuren- und Ortskarten zeigen Bilder sowie zentrale Informationen direkt in der Übersicht.
- Verknüpfungszahlen zu Ideen, Recherche und Inspiration bleiben bei Figuren sichtbar.
- Damit lassen sich auch umfangreichere Casts und Welten übersichtlicher verwalten.

### 💾 Datensicherheit

- Figuren- und Ortsakten werden weiterhin direkt im jeweiligen Romanprojekt in IndexedDB gespeichert.
- Hochgeladene Bilder werden vor dem Speichern verkleinert, um die lokale Datenmenge zu begrenzen.
- Bestehende ältere Figuren und Orte bleiben kompatibel und können nach und nach um die neuen Felder ergänzt werden.
- Alle neuen Aktenfelder und Bilder sind automatisch Bestandteil der Autoren-Suite-Backups.

## v0.22.0

### 🔗 Modulübergreifende Verknüpfungen

- Die bisher getrennten Bereiche der Autoren-Suite wurden deutlich stärker miteinander verbunden.
- Figuren, Namenslisten, Ideenzettel, Recherche, Inspiration und Manuskriptszenen können jetzt direkt aufeinander verweisen.
- Verknüpfungen verändern die ursprünglichen Inhalte nicht und bleiben als eigenständige Datensätze erhalten.

### 🎭 Figuren & Namenslisten

- Figuren im Romanprojekt können direkt mit einem Eintrag aus den Namenslisten verknüpft werden.
- Bei einer neuen Figur kann der verknüpfte Name automatisch als Figurenname übernommen werden.
- Ein verwendeter Namenseintrag wird automatisch als „verwendet“ markiert.
- Projektübergreifende Namen können beim Verknüpfen automatisch dem betreffenden Romanprojekt zugeordnet werden.
- Wird die letzte Figurenverknüpfung eines Namens entfernt, kann der Eintrag wieder als frei geführt werden.

### 💡 Figuren & Ideen

- Figuren können direkt mit beliebig vielen Ideenzetteln verbunden werden.
- Das Ideenarchiv wird dafür innerhalb des Figurendialogs durchsucht.
- Verknüpfte Zettel bleiben weiterhin vollständig im zentralen Ideenarchiv erhalten.

### ◉ Figuren & Recherche

- Rechercheeinträge können einzelnen Figuren zugeordnet werden.
- Projektbezogene und projektübergreifende Recherche steht dabei zur Verfügung.
- Dadurch können beispielsweise historische Quellen, Kleidungsreferenzen oder Hintergrundmaterial direkt bei der Figur gebündelt werden.

### ♧ Figuren & Inspiration

- Figuren können mit Bildern, Musik, Farben und anderen Inspirationseinträgen verknüpft werden.
- Die Inspiration bleibt eigenständig im Moodboard erhalten.
- Figurenspezifische Inspirationssammlungen können damit aufgebaut werden, ohne Inhalte zu duplizieren.

### ✒️ Szenen & Inspiration

- Der Szenen-Inspector unterstützt jetzt neben Ideenzetteln und Recherche auch Inspiration.
- Projektbezogene und projektübergreifende Inspiration kann direkt im Inspector gesucht und einer Szene zugeordnet werden.
- Verknüpfte Bilder und andere Inspirationsinhalte bleiben direkt neben dem Manuskript sichtbar.
- Beim Zusammenführen von Manuskriptabschnitten werden Inspirationsverknüpfungen mit übernommen.

### 🕸️ Verknüpfungsübersicht im Romanprojekt

- Romanprojekte besitzen jetzt einen eigenen Tab „Verknüpfungen“.
- Dort wird sichtbar, welche Figuren mit Namen, Ideen, Recherche und Inspiration verbunden sind.
- Zusätzlich zeigt die Übersicht für Manuskriptabschnitte die Anzahl verknüpfter Ideen, Rechercheeinträge, Inspirationen und Figuren.
- Damit lässt sich die wachsende Struktur eines Romans zentral überblicken.

### 💾 Datensicherheit

- Alle neuen Verknüpfungen werden in den bestehenden Projekt- und Manuskriptdatensätzen gespeichert.
- Es werden keine Inhalte durch Verknüpfungen verschoben oder gelöscht.
- Beim Zusammenführen von Szenen bleiben bestehende Verknüpfungen erhalten.
- Die neuen Beziehungen sind automatisch Bestandteil der Autoren-Suite-Backups.

## v0.21.0

### ☷ Namenslisten

- „Namenslisten“ ist jetzt als eigener vollständig nutzbarer Bereich der Autoren-Suite verfügbar.
- Vor-, Nach-, Orts-, Fantasie- und sonstige Namen können getrennt gesammelt werden.
- Jeder Name kann Herkunft, Bedeutung, Notizen, Schlagwörter und eine eigene Sammlung erhalten.
- Namen können projektübergreifend bleiben oder direkt einem Romanprojekt zugeordnet werden.

### ⌕ Suche & Filter

- Die Namenssuche berücksichtigt Name, Herkunft, Bedeutung, Notizen, Sammlung und Schlagwörter.
- Nach Namensart und Romanprojekt kann gefiltert werden.
- Schnellfilter zeigen alle Namen, Favoriten, noch freie Namen oder bereits verwendete Namen.
- Die Ergebnisliste wird alphabetisch sortiert.

### ♡ Favoriten & Verwendung

- Namen können direkt in der Übersicht als Favorit markiert werden.
- Bereits verwendete Namen können gekennzeichnet werden.
- Dadurch lässt sich schnell erkennen, welche Namen noch für neue Figuren, Orte oder Projekte frei sind.

### ⚄ Zufallsname

- Aus der aktuell gefilterten Namensmenge kann ein Zufallsname gezogen werden.
- Typ, Herkunft, Sammlung und Projektbezug bleiben beim Zufallsfund sichtbar.
- Mit „Nochmal“ kann direkt ein weiterer passender Name gezogen werden.
- Der gezogene Name kann unmittelbar geöffnet und bearbeitet werden.

### ≡ Schnelleingabe

- Ganze Namenslisten können auf einmal eingefügt werden.
- Ein Name pro Zeile wird automatisch als eigener Datensatz angelegt.
- Art, Sammlung und Romanprojekt können für die gesamte eingefügte Liste vorgegeben werden.
- Leere Zeilen und bereits vorhandene Namen werden beim Import übersprungen.
- Dadurch können bestehende Namenssammlungen deutlich schneller übernommen werden.

### 💾 Datensicherheit

- Namen werden im bereits vorbereiteten eigenen IndexedDB-Speicher abgelegt.
- Namensdaten sind Bestandteil der bestehenden Autoren-Suite-Backups.
- Bestehende Ideen-, Projekt-, Manuskript-, Recherche-, Inspirations- und Statistikdaten bleiben unverändert erhalten.

## v0.20.0

### 📊 Schreibstatistiken

- „Statistiken“ ist jetzt als eigener vollständig nutzbarer Bereich der Autoren-Suite verfügbar.
- Automatisch erfasste Schreibsessions und manuelle Wortzahl-Einträge werden gemeinsam ausgewertet.
- Die Statistik kann projektübergreifend oder für ein einzelnes Romanprojekt betrachtet werden.
- Auswertungszeiträume von 7, 30, 90 und 365 Tagen sowie der gesamte erfasste Zeitraum stehen zur Verfügung.

### ✍️ Wortfortschritt

- Geschriebene Wörter für heute und die letzten sieben Tage werden direkt hervorgehoben.
- Der Wortfortschritt des gewählten Zeitraums wird als Tagesverlauf visualisiert.
- Manuelle Einträge und automatisch erfasste Editor-Sessions bleiben unterscheidbar.
- Tage mit beiden Arten von Einträgen werden gemeinsam ausgewertet.
- Negative Nettoänderungen durch Überarbeitung bleiben in den zugrunde liegenden Daten erhalten.

### ⏱️ Schreibzeit & Rhythmus

- Die im Editor erfasste Schreibzeit wird für den gewählten Zeitraum summiert.
- Anzahl der Schreibsessions und Schreibtage werden ausgewertet.
- Der durchschnittliche Wortfortschritt pro aktivem Schreibtag wird berechnet.
- Der beste Schreibtag im gewählten Zeitraum wird angezeigt.
- Eine aktuelle Schreibserie zeigt aufeinanderfolgende Tage mit Schreibaktivität.

### 📚 Projekte

- Die aktuelle Manuskriptwortzahl aller Romanprojekte wird miteinander verglichen.
- Wortstände werden projektbezogen als Fortschrittsbalken dargestellt.
- Die gesamte aktuelle Manuskriptwortzahl ist unabhängig von den historischen Schreibaktivitäten sichtbar.

### 🗓️ Schreibkalender

- Ein Monatskalender macht sichtbar, an welchen Tagen geschrieben wurde.
- Die Intensität eines Tages richtet sich nach der geschriebenen Wortmenge.
- Dadurch wird der persönliche Schreibrhythmus zusätzlich visuell erkennbar.

### 🕘 Aktivitätsverlauf

- Die letzten Schreibaktivitäten werden mit Datum, Wortveränderung und Romanprojekt aufgelistet.
- Automatische Schreibsessions zeigen zusätzlich den betroffenen Manuskriptabschnitt und die erfasste Schreibdauer.
- Manuelle Wortzahl-Einträge werden separat gekennzeichnet und zeigen vorhandene Notizen.
- Damit lässt sich nachvollziehen, was wann und an welchem Projekt geschrieben wurde.

### ➕ Manuelle Einträge

- Die bereits vorhandene NaNoWriMo-artige Wortzahl-Eingabe ist direkt aus der Statistik erreichbar.
- Neue manuelle Einträge aktualisieren die Statistik unmittelbar.
- Auch außerhalb der Autoren-Suite geschriebene Wörter können dadurch in der persönlichen Schreibchronik berücksichtigt werden.

## v0.19.0

### ♧ Inspiration & Moodboards

- „Inspiration“ ist jetzt als eigener nutzbarer Bereich der Autoren-Suite verfügbar.
- Bilder, Musik, Textschnipsel, Farben, Links und freie Inspirationsnotizen können zentral gesammelt werden.
- Inspiration ist bewusst von der sachlicheren Recherche getrennt und darf stärker visuell, emotional und assoziativ funktionieren.
- Einträge können projektübergreifend bleiben oder direkt einem Romanprojekt zugeordnet werden.
- Suche sowie Filter nach Typ und Romanprojekt erleichtern die Arbeit mit größeren Inspirationssammlungen.

### 🖼️ Visuelle Sammlung

- Bilder können direkt als Inspirationsmaterial hochgeladen werden.
- Hochgeladene Bilder werden automatisch verkleinert und lokal gespeichert.
- Die Inspirationsübersicht verwendet eine freie, unterschiedlich hohe Kartenanordnung, die an ein digitales Moodboard erinnert.
- Farben können als eigene Inspirationskarten gespeichert und großflächig dargestellt werden.
- Bild- und Farbkarten stehen gleichberechtigt neben Text- und Musikinspirationen.

### ♫ Musik, Texte & Links

- Musik kann als eigener Inspirationstyp mit Link gespeichert werden.
- Text- und Zitatschnipsel erhalten eine bewusst stärker literarische Darstellung.
- Lose Links und freie Notizen können ebenfalls gesammelt werden.
- Stichwörter und Stimmungen können jedem Eintrag unabhängig vom Typ hinzugefügt werden.

### 🧭 Projektbezug

- Inspirationen können direkt einem Romanprojekt zugeordnet werden.
- Projektübergreifende Mood- und Ideensammlungen bleiben weiterhin möglich.
- Die Projektzuordnung wird direkt auf den Inspirationskarten angezeigt.

### 💾 Datensicherheit

- Inspirationen werden im bereits vorbereiteten eigenen IndexedDB-Speicher abgelegt.
- Bilder werden lokal im Browser gespeichert.
- Inspirationsdaten sind Bestandteil der bestehenden Autoren-Suite-Backups.
- Bestehende Ideen-, Recherche- und Manuskriptdaten bleiben beim Wechsel auf v0.19.0 unverändert.

## v0.18.0

### ◉ Recherche & Medien

- „Recherche“ ist jetzt als eigener nutzbarer Bereich der Autoren-Suite verfügbar.
- Quellen, Bilder, Links, Musik, Videos und freie Recherche-Notizen können zentral gesammelt werden.
- Rechercheeinträge können projektübergreifend oder direkt einem Romanprojekt zugeordnet werden.
- Suche sowie Filter nach Typ und Romanprojekt erleichtern die Arbeit mit wachsenden Recherchesammlungen.
- Recherche wird in einer visuellen Kartenansicht dargestellt, die sich in die bestehende Papier- und Schreibtischästhetik einfügt.

### 🖼️ Bilder

- Eigene Bilder können direkt in einen Rechercheeintrag geladen werden.
- Hochgeladene Bilder werden vor der lokalen Speicherung automatisch auf eine sinnvolle Maximalgröße verkleinert.
- Dadurch bleibt die lokale Datenmenge auch bei umfangreicheren Bildsammlungen besser kontrollierbar.
- Bilder werden in der Kartenübersicht und in einer größeren Detailansicht dargestellt.
- Bilddaten werden gemeinsam mit dem Rechercheeintrag in IndexedDB gespeichert und in Backups übernommen.

### 🔗 Links, Musik & Videos

- Weblinks können als eigene Rechercheeinträge gespeichert werden.
- Musik- und Videoquellen besitzen eigene Inhaltstypen.
- YouTube-Links können in der Detailansicht direkt als eingebettetes Video angezeigt werden.
- Andere externe Quellen bleiben als sicher öffnende Links erhalten.
- Notizen und Schlagwörter können unabhängig vom Medientyp ergänzt werden.

### 🧭 Verbindung mit Romanprojekten

- Recherche kann einem konkreten Romanprojekt zugeordnet werden.
- Projektübergreifende Recherche bleibt zusätzlich möglich und kann für mehrere Bücher relevant sein.
- Der Recherchebereich zeigt auf jeder Karte die jeweilige Projektzuordnung.

### ✒️ Recherche im Szenen-Inspector

- Rechercheeinträge können direkt mit einem einzelnen Kapitel oder einer Szene verknüpft werden.
- Der Inspector durchsucht projektbezogene und projektübergreifende Recherche.
- Verknüpfte Recherche wird direkt neben dem Manuskript angezeigt.
- Verknüpfungen können hinzugefügt und wieder entfernt werden, ohne den Rechercheeintrag selbst zu verändern.
- Beim Zusammenführen von Manuskriptabschnitten werden Recherche-Verknüpfungen mit übernommen.

### 💾 Datensicherheit

- Rechercheeinträge werden im bereits vorbereiteten eigenen IndexedDB-Speicher abgelegt.
- Bilddaten werden lokal gespeichert und verlassen für die Grundfunktion nicht den Browser.
- Gelöschte Recherche-Verknüpfungen werden automatisch aus betroffenen Manuskriptabschnitten entfernt.
- Recherche und Medien sind Bestandteil der bestehenden Autoren-Suite-Backups.

## v0.17.0

### 🧭 Szenen-Inspector

- Der Schreibeditor besitzt jetzt einen echten Inspector für den aktuell geöffneten Manuskriptabschnitt.
- Der Inspector ist in die Bereiche „Szene“, „Projekt“ und „Verknüpft“ gegliedert.
- Szeneninformationen bleiben direkt neben dem Manuskript sichtbar, ohne den Schreibfluss zu unterbrechen.

### 🎭 Szenendaten

- Für jeden Kapitel- oder Szenenabschnitt kann ein Kurzinhalt hinterlegt werden.
- Eine POV-Figur kann aus den im Romanprojekt angelegten Figuren gewählt werden.
- Ein Ort kann direkt aus der Ortsplanung des Projekts zugeordnet werden.
- Freie Zeitangaben wie Tageszeit, Tag im Handlungsverlauf oder Zeitsprung können gespeichert werden.
- Ziel und Konflikt des aktuellen Abschnitts besitzen ein eigenes Feld.
- Beliebig viele im Projekt angelegte Figuren können als in der Szene auftretend markiert werden.
- Die bereits vorhandene freie Szenennotiz bleibt zusätzlich erhalten.

### 🔗 Verknüpfte Inhalte

- Projektspezifische Ideenzettel können jetzt direkt mit einem einzelnen Manuskriptabschnitt verknüpft werden.
- Der Inspector durchsucht dafür die bereits zum Romanprojekt gehörenden Ideenzettel.
- Verknüpfungen können direkt hinzugefügt oder wieder entfernt werden.
- Planungsblöcke, aus denen ein Manuskriptabschnitt bei der Übergabe entstanden ist, werden im Inspector als Planungsgrundlage angezeigt.
- Der Bereich für spätere Recherche-Verknüpfungen ist bereits in die Inspector-Struktur integriert.

### 🗂️ Projekt im Blick

- Prämisse, Stimmung, Figuren, Orte und Planungsnotizen des Gesamtprojekts bleiben über einen eigenen Inspector-Tab erreichbar.
- Dadurch kann zwischen szenenspezifischen Details und der großen Projektperspektive gewechselt werden.

### 💾 Autosave & Sicherheit

- Inspector-Daten werden automatisch gemeinsam mit dem aktuellen Manuskriptabschnitt gespeichert.
- Kurzinhalt, POV, Ort, Zeit, Ziel/Konflikt, Figuren und Notizen werden von der bestehenden Notfallsicherung berücksichtigt.
- Beim Zusammenführen zweier Manuskriptabschnitte werden Szenendaten möglichst verlustfrei zusammengeführt.
- Ideenzettel- und Planungsverknüpfungen bleiben beim Zusammenführen erhalten.
- Alle Inspector-Daten werden automatisch in den bestehenden Autoren-Suite-Backups gesichert.

## v0.16.0

### ✒️ Romaneditor

- Der bisherige einfache Schreibbereich wurde zu einem Rich-Text-Romaneditor erweitert.
- Manuskripttexte können jetzt direkt formatiert werden.
- Vorhandene reine Textabschnitte werden beim Öffnen automatisch editorfreundlich dargestellt und beim nächsten Speichern sicher in das neue Format überführt.
- Die bestehende mehrstufige Autosave- und Notfallsicherung bleibt erhalten.

### 🎨 Textformatierung

- Fließtext, Kapitelüberschriften, Zwischenüberschriften und Zitate stehen als Absatzformate zur Verfügung.
- Fett, Kursiv und Unterstrichen können direkt über die Editorleiste gesetzt werden.
- Aufzählungen und nummerierte Listen werden unterstützt.
- Szenentrenner können mit einer eigenen Aktion direkt in den Text eingefügt werden.
- Der Fokusmodus bleibt auch mit der neuen Editorleiste nutzbar.

### ⌕ Suchen & Ersetzen

- Der Manuskripteditor besitzt jetzt eine eigene Suche.
- `Strg/Cmd + F` öffnet die interne Suchen-und-Ersetzen-Leiste.
- Alle Treffer im aktuellen Manuskriptabschnitt werden markiert.
- Zwischen Treffern kann vor- und zurückgesprungen werden.
- Einzelne Treffer oder alle Treffer können ersetzt werden.
- Die Wort- und Zeichenzählung wird nach Änderungen direkt aktualisiert.

### ✂️ Manuskript teilen

- Kapitel und Szenen können direkt an der aktuellen Cursorposition geteilt werden.
- Der Text vor der Cursorposition bleibt im bisherigen Abschnitt.
- Der Text danach wird in einen neuen Manuskriptabschnitt übernommen.
- Titel und Typ des neuen Abschnitts können beim Teilen festgelegt werden.
- Vor dem Teilen wird automatisch eine Sicherheitsversion des ursprünglichen Textes erstellt.

### 🔗 Abschnitte zusammenführen

- Der aktuelle Manuskriptabschnitt kann mit dem vorherigen Abschnitt zusammengeführt werden.
- Die Texte werden in ihrer Reihenfolge sicher kombiniert.
- Szenennotizen und Verknüpfungen zur Planungswand werden beim Zusammenführen mitgenommen.
- Vor dem Zusammenführen werden Sicherheitsversionen beider Ausgangstexte angelegt.
- Erst nach erfolgreicher Sicherung wird der nicht mehr benötigte Ausgangsabschnitt entfernt.

### 🗂️ Binder & Workflow

- Der vorhandene Kapitel-/Szenen-Binder bleibt vollständig mit dem neuen Romaneditor verbunden.
- Kapitel und Szenen können weiterhin hierarchisch organisiert und per Drag & Drop sortiert werden.
- Teilen und Zusammenführen ermöglichen jetzt eine immer feinere Manuskriptstruktur direkt während des Schreibprozesses.
- Die Verbindung zwischen Planungswand, Manuskriptstruktur und Schreibeditor bleibt erhalten.

### 🛡️ Schreibsicherheit

- Rich-Text-Inhalte werden ebenfalls von Notfallsicherung, Autosave und Sicherheitsversionen erfasst.
- `Strg/Cmd + S` kann zusätzlich jederzeit eine sofortige Speicherung anstoßen.
- Teilen und Zusammenführen sind bewusst nicht-destruktiv vorbereitet: Vor strukturellen Änderungen werden ältere Textstände gesichert.
- Bestehende Manuskripte werden nicht automatisch überschrieben oder neu aufgebaut.

## v0.15.0

### 🧩 Planungswand

- Romanprojekte besitzen jetzt eine eigene Planungswand zwischen Ideenarchiv und Manuskript.
- Projektspezifische Ideenzettel können direkt aus dem gesamten Ideenarchiv gesucht und auf die Planungswand gelegt werden.
- Ein verwendeter Zettel bleibt weiterhin vollständig im zentralen Ideenarchiv erhalten.
- Beim Hinzufügen wird der Zettel gleichzeitig mit dem jeweiligen Romanprojekt verknüpft.
- Bereits verknüpfte Zettel werden bei der Suche entsprechend gekennzeichnet.

### ↕️ Freie Reihenfolge

- Ideenzettel und eigene Planungsblöcke können per Drag & Drop in eine beliebige Reihenfolge gebracht werden.
- Die Reihenfolge wird dauerhaft innerhalb des Romanprojekts gespeichert.
- Die Planung kann dadurch zunächst vollständig ohne Kapitelstruktur entstehen und schrittweise feiner werden.

### 📝 Eigene Textblöcke

- Zwischen vorhandenen Ideenzetteln können beliebig viele freie Textblöcke eingefügt werden.
- Textblöcke können Überschrift und ausführlichen Planungstext enthalten.
- Ideenzettel können für das konkrete Romanprojekt um zusätzliche Planung ergänzt werden.
- Diese projektspezifische Erweiterung verändert den ursprünglichen Zettel im Ideenarchiv nicht.

### ✂️ Kapitel slicen & zusammenführen

- Kapiteltrenner können an beliebigen Stellen der Planungswand eingefügt werden.
- Direkt an einem Planungsblock kann mit der Slice-Aktion ein neues Kapitel davor begonnen werden.
- Kapiteltrenner lassen sich frei verschieben und umbenennen.
- Wird ein Kapiteltrenner entfernt, werden die angrenzenden Planungsabschnitte wieder zusammengeführt.
- Dadurch muss die Kapitelaufteilung nicht früh feststehen und kann jederzeit verfeinert oder wieder gröber gemacht werden.

### ✎ Übergabe an den Schreibeditor

- Die aktuelle Planungswand kann direkt in das Manuskript übergeben werden.
- Kapiteltrenner bestimmen dabei die entstehenden Manuskriptkapitel.
- Ideenzettel, projektspezifische Erweiterungen und freie Textblöcke werden in der festgelegten Reihenfolge als Ausgangstext übernommen.
- Die verwendeten Planungsblöcke bleiben mit den erzeugten Manuskriptabschnitten technisch verknüpft.
- Bereits vorhandene Manuskriptabschnitte werden nicht überschrieben; neue Inhalte werden sicher ergänzt.
- Nach der Übergabe öffnet sich direkt der Schreibeditor des betreffenden Romanprojekts.

### 🛡️ Datensicherheit

- Die Planungswand wird direkt im jeweiligen Romanprojekt in IndexedDB gespeichert.
- Die Übergabe an den Editor überschreibt keine vorhandenen Manuskripttexte.
- Planungsdaten werden weiterhin vollständig von den Autoren-Suite-Backups erfasst.
- Die mehrstufige Textsicherung des Schreibeditors bleibt unverändert erhalten.

## v0.14.0

### 🗂️ Kapitel & Szenen

- Manuskripte können jetzt hierarchisch aus Kapiteln und untergeordneten Szenen aufgebaut werden.
- Szenen können einem Kapitel zugeordnet oder ohne Kapitel geführt werden.
- Kapitel und Szenen werden in der Manuskriptleiste entsprechend eingerückt dargestellt.
- Manuskriptabschnitte können per Drag & Drop neu sortiert werden.
- Szenen können per Drag & Drop direkt einem Kapitel zugeordnet werden.
- Die Reihenfolge und Zuordnung werden dauerhaft im jeweiligen Romanprojekt gespeichert.

### 🚦 Szenenstatus

- Kapitel und Szenen besitzen jetzt einen eigenen Arbeitsstatus.
- Verfügbare Status: „Idee“, „Geplant“, „Rohfassung“, „Überarbeiten“ und „Fertig“.
- Der Status ist direkt in der Manuskriptleiste sichtbar.
- Im Schreibeditor kann der Status ohne Öffnen eines zusätzlichen Dialogs geändert werden.

### 📝 Szenennotizen

- Jeder Manuskriptabschnitt besitzt weiterhin eine eigene Notiz.
- Die Notiz ist jetzt direkt während des Schreibens in der rechten Kontextleiste verfügbar.
- Änderungen an Szenennotizen werden automatisch gespeichert.
- Szenennotizen werden gemeinsam mit dem Manuskriptabschnitt gesichert.

### ✍️ Manuelle Wortzahlen

- Schreibfortschritt kann jetzt zusätzlich manuell eingetragen werden.
- Unterstützt werden „Heute geschrieben“ und „Manuskript steht jetzt bei“.
- Einträge können einem Romanprojekt und einem Datum zugeordnet werden.
- Optionale Notizen ermöglichen beispielsweise Hinweise auf abgeschlossene Kapitel oder externe Schreibsessions.
- Die manuellen Einträge werden separat gespeichert und stehen für die kommenden Schreibstatistiken zur Verfügung.

### 🛡️ Schutz vor Textverlust

- Der Schreibeditor besitzt jetzt eine zusätzliche sofortige lokale Notfallsicherung.
- Während des Tippens wird der aktuelle Text synchron als lokaler Sicherheitsentwurf gespeichert, noch bevor das reguläre Autosave abgeschlossen ist.
- Das reguläre Autosave wurde beschleunigt und speichert nach kurzer Schreibpause in IndexedDB.
- Zusätzlich erfolgt während des Schreibens regelmäßig eine automatische Sicherung.
- Beim Wechseln von Abschnitten, beim Verlassen des Tabs und vor dem Schließen der Seite wird eine zusätzliche Speicherung angestoßen.
- Wird nach einem unerwarteten Abbruch ein neuerer lokaler Entwurf gefunden, bietet die Anwendung beim nächsten Öffnen die Wiederherstellung an.
- Nach erfolgreicher dauerhafter Speicherung wird die Notfallsicherung automatisch bereinigt.

### 🕘 Sicherheitsversionen

- Manuskriptabschnitte erhalten im Hintergrund zusätzliche Sicherheitsversionen.
- Während längerer Schreibphasen werden in regelmäßigen Abständen ältere Textstände aufbewahrt.
- Vor dem Löschen eines Manuskriptabschnitts wird automatisch eine Sicherheitsversion erstellt.
- Pro Abschnitt werden die jüngsten Sicherheitsstände begrenzt aufbewahrt, damit die lokale Datenbank nicht unbegrenzt wächst.
- Sicherheitsversionen werden zusammen mit der restlichen Autoren-Suite im Backup berücksichtigt.

## v0.13.0

### ✎ Schreibeditor

- „Schreiben“ ist jetzt als echter Arbeitsbereich der Autoren-Suite nutzbar.
- Der Schreibbereich arbeitet direkt mit den vorhandenen Romanprojekten zusammen.
- Zwischen Romanprojekten kann direkt im Editor gewechselt werden.
- Jedes Projekt besitzt ein eigenes Manuskript mit beliebig vielen Kapiteln und Szenen.
- Manuskriptabschnitte können angelegt, benannt, bearbeitet und wieder gelöscht werden.
- Titel und Text eines Abschnitts werden direkt im Schreibbereich bearbeitet.

### 💾 Autosave

- Änderungen am Manuskript werden automatisch nach kurzer Schreibpause gespeichert.
- Der aktuelle Speicherstatus wird direkt im Editor angezeigt.
- Wortzahlen der einzelnen Abschnitte werden mitgespeichert.
- Der Gesamtwortstand eines Romanprojekts wird automatisch aus allen Manuskriptabschnitten berechnet.
- Der aktuelle Wortstand wird gleichzeitig in das zugehörige Romanprojekt übernommen.

### 📊 Wortzahlen & Schreibsessions

- Wort- und Zeichenzahl des aktuellen Abschnitts werden live angezeigt.
- Die Gesamtwortzahl des Manuskripts ist jederzeit sichtbar.
- Ein vorhandenes Projekt-Wortziel wird als Fortschrittsanzeige im Schreibbereich dargestellt.
- Beim Schreiben wird im Hintergrund eine Schreibsession begonnen.
- Schreibsessions speichern Start- und Endwortzahl, Nettoveränderung und Dauer als Grundlage für die späteren ausführlichen Schreibstatistiken.
- Die Nettoveränderung der aktuellen Session wird bereits während des Schreibens angezeigt.

### 🗂️ Manuskriptnavigation

- Kapitel und Szenen erscheinen in einer eigenen linken Manuskriptleiste.
- Zu jedem Abschnitt werden Typ und aktuelle Wortzahl angezeigt.
- Ein Klick wechselt direkt zwischen den Manuskriptabschnitten.
- Die Manuskriptleiste zeigt zusätzlich die Gesamtwortzahl des Projekts.

### 📝 Projektkontext

- Während des Schreibens bleibt das zugehörige Romanprojekt in einer eigenen Kontextleiste sichtbar.
- Prämisse, Stimmung, Figuren, Orte und Planungsnotizen können beim Schreiben überblickt werden.
- Dadurch muss für zentrale Planungsinformationen nicht ständig in die Projektverwaltung zurückgewechselt werden.

### ◫ Fokusmodus

- Der Schreibeditor besitzt einen eigenen Fokusmodus.
- Im Fokusmodus werden Manuskriptnavigation und Projektkontext ausgeblendet.
- Der eigentliche Textbereich wird verbreitert und ruhiger dargestellt.
- Der Fokusmodus kann jederzeit wieder verlassen werden.

## v0.12.0

### ▤ Romanprojekte

- „Romanprojekte“ ist jetzt als eigene Planungszentrale für konkrete Bücher nutzbar.
- Projekte können direkt angelegt oder aus einer bestehenden Romanidee erzeugt werden.
- Arbeitstitel, Status, Genre, Prämisse, Stimmung und Zielwortzahl werden zentral im Projekt verwaltet.
- Projekte besitzen die Status „Planung“, „Schreiben“, „Überarbeitung“, „Pausiert“ und „Abgeschlossen“.
- Eine eigene Projektübersicht zeigt alle Bücher als papierartige Projektkarten mit Planungsstand und Wortziel.

### 🗂️ Projektzentrale

- Jedes Romanprojekt besitzt einen eigenen Arbeitsbereich.
- Die Projektzentrale gliedert sich in Übersicht, Kapitel & Szenen, Figuren, Orte, Planungsnotizen und Ideenzettel.
- Die Übersicht zeigt zentrale Projektdaten und den aktuellen Umfang der einzelnen Planungsbereiche.
- Zielwortzahl und aktueller Wortstand sind bereits als Grundlage für den kommenden Schreibbereich vorbereitet.

### 🧱 Planung

- Kapitel und Szenen können als erste Strukturelemente angelegt und bearbeitet werden.
- Figuren erhalten einen eigenen Planungsbereich mit Rolle und freien Notizen.
- Orte können separat mit Bedeutung und Detailnotizen gesammelt werden.
- Freie Planungsnotizen lassen sich unabhängig von Figuren und Struktur verwalten.
- Alle Planungselemente werden direkt innerhalb des jeweiligen Romanprojekts gespeichert.

### 🔗 Verknüpfungen

- Beim Überführen einer Romanidee werden Prämisse, Genre, Stimmung und verknüpfte Ideenzettel übernommen.
- Die Verbindung zwischen ursprünglicher Romanidee und Romanprojekt bleibt erhalten.
- Übernommene Ideenzettel werden in der Projektzentrale gemeinsam angezeigt.
- Wird ein Projekt gelöscht, kann die ursprüngliche Romanidee später erneut in ein Projekt überführt werden.

### ✎ Vorbereitung Schreiben

- Romanprojekte besitzen bereits eine direkte Aktion zum zukünftigen Schreibbereich.
- Projekt-ID, Wortziel, Struktur und Planungsdaten bilden die Grundlage für Manuskript, Kapitel, Szenen und Schreibstatistiken der nächsten Ausbaustufe.

## v0.11.0

### ▱ Romanideen

- „Romanideen“ ist jetzt der zweite vollständig nutzbare Bereich der Autoren-Suite.
- Große Romanideen werden bewusst getrennt von den kleinen Ideenzetteln verwaltet.
- Neue Romanideen können mit Arbeitstitel, Status, Genre, Prämisse, Stimmung und freien Notizen angelegt werden.
- Romanideen werden als eigene papierartige Dossier-Karten dargestellt.
- Suche, Statusfilter und Sortierung erleichtern die Verwaltung wachsender Sammlungen.
- Vier Entwicklungsstände stehen zur Verfügung: „Funke“, „Sammeln“, „Entwickeln“ und „Bereit fürs Projekt“.

### 🔗 Ideenzettel verknüpfen

- Vorhandene Zettel aus dem Ideenarchiv können direkt mit einer Romanidee verknüpft werden.
- Die Zettel bleiben weiterhin eigenständig im Ideenarchiv erhalten.
- Innerhalb der Romanidee können verknüpfte Zettel gesucht, hinzugefügt und wieder entfernt werden.
- In der Detailansicht einer Romanidee werden die zugehörigen Zettel gemeinsam angezeigt.

### ▤ Übergang zum Romanprojekt

- Eine Romanidee kann jetzt direkt in ein neues Romanprojekt überführt werden.
- Arbeitstitel, Prämisse, Genre, Stimmung und verknüpfte Ideenzettel werden dabei als Ausgangsdaten übernommen.
- Die ursprüngliche Romanidee bleibt erhalten und wird mit dem erzeugten Projekt verknüpft.
- Bereits überführte Romanideen werden entsprechend gekennzeichnet und nicht versehentlich mehrfach konvertiert.

### 💾 Daten & Backup

- Romanideen werden im bereits vorbereiteten eigenen IndexedDB-Bereich gespeichert.
- Bestehende Ideenarchiv-Daten bleiben unverändert.
- Romanideen und ihre Verknüpfungen werden automatisch in die Autoren-Suite-Backups aufgenommen.

## v0.10.0

### ✒️ Autoren-Suite

- Das bisherige Ideenarchiv wurde technisch als erster Bestandteil einer größeren Autoren-Suite eingeordnet.
- Die Hauptnavigation wurde um die zentralen zukünftigen Arbeitsbereiche erweitert.
- Romanideen, Romanprojekte, Schreiben, Namenslisten, Recherche, Inspiration, Statistiken und Einstellungen besitzen jetzt eigene vorbereitete Modulansichten.
- Zusätzlich wurden „Romanprojekte“ und „Schreiben“ als feste Hauptbereiche in die Navigation aufgenommen.
- Beim Wechsel in einen Suite-Bereich passt sich die linke Navigation an den jeweiligen Arbeitskontext an.

### 🧩 Modulstruktur

- Für jeden zukünftigen Bereich wurde eine eigene technische Modulgrundlage vorbereitet.
- Die Modulansichten zeigen bereits Zweck, geplante Verknüpfungen, Datengrundlage und den vorgesehenen nächsten Ausbau.
- Die grundlegende Verbindung `Idee → Romanidee → Romanprojekt → Manuskript → Statistik` ist jetzt als Architekturprinzip in der Anwendung sichtbar.
- Die einzelnen Module werden bewusst nicht als voneinander getrennte Programme aufgebaut, sondern sollen Daten untereinander verknüpfen können.

### 💾 Datenarchitektur

- Die lokale IndexedDB-Struktur wurde für die zukünftigen Suite-Bereiche erweitert.
- Separate Datenspeicher für Romanideen, Projekte, Dokumente, Namenslisten, Recherche, Inspiration, Schreibsessions und Medien wurden vorbereitet.
- Bestehende Ideen bleiben unverändert im bisherigen Ideenspeicher erhalten.
- Die Datenbank wird beim Wechsel auf v0.10.0 automatisch erweitert, ohne bestehende Zettel zu löschen.

### 🔐 Backup

- Backups sind jetzt auf die gesamte zukünftige Autoren-Suite vorbereitet.
- Neben Ideen, Kategorien und Schnellregistern werden auch Daten der neuen Modulbereiche gemeinsam gesichert.
- Bestehende ältere Ideenarchiv-Backups bleiben weiterhin importierbar.
- Neue Sicherungen werden als Autoren-Suite-Backup gekennzeichnet.

## v0.9.0

### ⚙️ Technische Aufräumrunde

- Die interne Verarbeitung großer Ideensammlungen wurde für deutlich höhere Datenmengen vorbereitet.
- Kategorien werden intern schneller aufgelöst und Kategorie-Zähler effizienter berechnet.
- Suchtexte werden zwischengespeichert, damit wiederholte Suchen in umfangreichen Archiven weniger Arbeit verursachen.
- Die Texteingabe in der Suche wird leicht verzögert ausgewertet, um unnötige Neuberechnungen während schnellen Tippens zu vermeiden.
- Die Dashboard-Navigation und -Auswertung wurden stabilisiert.

### ⌨️ Bedienung

- Tastenkürzel für häufige Aktionen ergänzt.
- `Strg + K` fokussiert die globale Suche.
- `Strg + N` öffnet eine neue Idee.
- `Strg + Shift + N` öffnet den Übertragungsmodus.
- `Strg + B` erstellt direkt ein Backup.
- Eine kleine Übersicht der verfügbaren Tastenkürzel ist direkt in der Anwendung erreichbar.

### 🎨 Oberfläche

- Leere Such- und Kategorienansichten wurden überarbeitet und bieten jetzt direkt eine Aktion zum Anlegen einer neuen Idee.
- Fokuszustände für Tastaturbedienung wurden deutlicher sichtbar gemacht.
- Das responsive Verhalten für schmalere Browserfenster wurde verbessert.
- Zettelansicht, Werkzeugleiste und untere Navigation passen sich besser an unterschiedliche Fensterbreiten an.

## v0.8.0

### 💾 Datensicherheit & Backup

- Der Backup-Bereich wurde zu einer eigenen Datensicherheitsfunktion ausgebaut.
- Das Archiv zeigt jetzt an, wann zuletzt ein Backup erstellt wurde.
- Bei fehlenden oder älteren Sicherungen wird dezent auf ein neues Backup hingewiesen.
- Backups enthalten Ideen, Kategorien, Kategorienreihenfolge und Schnellregister gemeinsam.
- Backup-Dateien enthalten zusätzlich Versions- und Exportinformationen.

### 🔎 Backup-Prüfung

- Backup-Dateien werden vor dem Import auf grundlegende Struktur und Vollständigkeit geprüft.
- Beschädigte oder unpassende Dateien werden nicht ungeprüft in das Archiv übernommen.
- Vor der Wiederherstellung werden Datum und Anzahl der enthaltenen Ideen angezeigt.

### ♻️ Wiederherstellung

- Backups können jetzt als vollständige Wiederherstellung verwendet werden.
- Bei einer vollständigen Wiederherstellung ersetzt das Backup bewusst den aktuellen lokalen Archivstand.
- Alternativ bleibt der bisherige ergänzende Import erhalten, bei dem Daten zum vorhandenen Archiv hinzugefügt werden.
- Beide Varianten verlangen vor der Übernahme eine ausdrückliche Bestätigung.

## v0.7.0

### 🎲 Zufall & Wiederentdecken

- Die bisherige Zufallsfunktion wurde zu einem eigenen Wiederentdecken-Modus ausgebaut.
- Es können wahlweise 1, 3, 5 oder 10 zufällige Ideenzettel gleichzeitig gezogen werden.
- Zufallsziehungen können aus dem gesamten Archiv, nur aus der aktuellen Kategorie oder aus der aktuellen Kategorie inklusive Unterkategorien erfolgen.
- Favorisierte Ideen können auf Wunsch von der Zufallsauswahl ausgeschlossen werden.
- Gezogene Ideen werden als eigene papierartige Zettelsammlung präsentiert.
- Einzelne gezogene Zettel können direkt geöffnet und weiterbearbeitet werden.
- Mit „Nochmal ziehen“ lässt sich unmittelbar eine neue Auswahl aus demselben Bereich erzeugen.
- Die bisherige kompakte Stöberansicht bleibt zusätzlich erhalten.

## v0.6.0

### 📊 Dashboard & Wachstum

- Das Ideenarchiv besitzt jetzt ein eigenes Dashboard.
- Die Gesamtzahl aller aktiven Ideen wird dort prominent dargestellt.
- Neue Zettel werden für heute, die letzten 7 Tage, die letzten 30 Tage und das laufende Jahr ausgewertet.
- Eine Wachstumsgrafik zeigt die Aktivität der vergangenen 30 Tage.
- Der nächste Archiv-Meilenstein wird automatisch berechnet und mit Fortschrittsanzeige dargestellt.
- Die größten Hauptkategorien werden inklusive Unterkategorien ausgewertet.
- Die zuletzt hinzugefügten Zettel erscheinen direkt im Dashboard und können von dort geöffnet werden.
- Das Dashboard übernimmt die bestehende Papier-, Pastell- und Schreibtischästhetik des Ideenarchivs.****

## v0.5.0

### 📥 Archiv übertragen

- Die bisherige Massenerfassung wurde zu einem eigenen Übertragungsmodus ausgebaut.
- Mehrere alte Notizen können gesammelt eingefügt und vor dem Speichern als einzelne Zettel geprüft werden.
- Aufzählungszeichen und nummerierte Listen werden beim Einfügen automatisch bereinigt.
- Eine Vorschau zeigt vor der Übernahme, wie viele Zettel erkannt wurden.
- Einzelne erkannte Ideen können vor dem Speichern gezielt abgewählt werden.

### 🔄 Duplikatkontrolle

- Exakt bereits vorhandene Ideen werden im Übertragungsmodus direkt gekennzeichnet.
- Möglicherweise ähnliche vorhandene Zettel werden separat hervorgehoben und zum Vergleich angezeigt.
- Exakte Duplikate können beim Übernehmen automatisch übersprungen werden.
- Die Duplikatkontrolle verhindert keine bewusste Mehrfachspeicherung und bleibt vollständig kontrollierbar.

### ✍️ Digitalisierung

- Die Zielkategorie wird weiterhin nur einmal für einen Übertragungsvorgang ausgewählt.
- Der Modus ist speziell auf das schrittweise Übertragen umfangreicher bestehender Notizsammlungen ausgelegt.
- Die Anzahl der erkannten, neuen und möglicherweise bereits vorhandenen Zettel wird vor dem Speichern sichtbar gemacht.

## v0.4.0

### 🗃️ Zettelverwaltung

- Ideenzettel können jetzt direkt in der Übersicht bearbeitet werden.
- Ein Doppelklick auf den Text oder der neue Bearbeiten-Button startet die Direktbearbeitung.
- Änderungen können ohne zusätzlichen Dialog gespeichert werden.
- Einzelne Ideen können jetzt direkt aus der Detailansicht dupliziert werden.
- Favoriten lassen sich direkt auf dem Zettel setzen oder entfernen.

### ☑️ Mehrfachauswahl

- Jeder Zettel besitzt jetzt eine sichtbare Auswahlmöglichkeit.
- Mehrere ausgewählte Ideen können gemeinsam in eine andere Kategorie verschoben werden.
- Ausgewählte Ideen können gemeinsam als Favoriten markiert werden.
- Mehrere Ideen können gesammelt in den Papierkorb verschoben werden.
- Die aktuelle Auswahl kann mit einer Aktion vollständig aufgehoben werden.
- Strg-Klick und Rechtsklick bleiben zusätzlich als schnelle Auswahlmöglichkeiten erhalten.

### ↪️ Drag & Drop

- Ideenzettel können per Drag & Drop direkt auf eine Kategorie in der linken Navigation gezogen werden.
- Ist der gezogene Zettel Teil einer Mehrfachauswahl, wird die gesamte Auswahl gemeinsam verschoben.
- Die Zielkategorie wird während des Ziehens visuell hervorgehoben.

### 📚 Kategorien

- Die in v0.3.0 eingeführte Register- und Reihenfolgenverwaltung ist jetzt vollständig im Kategorie-Dialog verfügbar.
- Kategorien können dort einem Schnellregister zugewiesen und innerhalb ihrer Ebene nach oben oder unten verschoben werden.

## v0.3.0

### 📑 Schnellregister

- Die farbigen Papierregister am linken Rand sind jetzt funktionale Schnellzugriffe.
- Jede Registerfarbe kann frei mit einer Kategorie belegt werden.
- Ein Klick auf ein belegtes Register öffnet direkt die zugewiesene Kategorie und klappt ihren Pfad im Kategorienbaum auf.
- Ein noch unbelegtes Register kann durch Anklicken direkt mit der aktuell geöffneten Kategorie verknüpft werden.
- Registerbelegungen können im Kategorie-Dialog geändert oder wieder entfernt werden.
- Das aktuell geöffnete Register wird visuell hervorgehoben.

### 📚 Kategorien

- Kategorien können innerhalb derselben Hierarchieebene nach oben oder unten verschoben werden.
- Die individuelle Reihenfolge wird dauerhaft gespeichert.
- Beim Verschieben einer Kategorie in eine andere übergeordnete Kategorie wird die Reihenfolge automatisch sauber angepasst.
- Beim Löschen einer Kategorie werden eventuell vorhandene Register-Verknüpfungen automatisch entfernt.

### 💾 Backup

- Schnellregister und individuelle Kategorienreihenfolge werden jetzt im Backup berücksichtigt.
- Vorhandene Archive und Ideen bleiben beim Wechsel auf v0.3.0 erhalten.

## v0.2.4

### 🎨 Zettelansicht

- Ein Layoutfehler behoben, durch den freie Höhe im Ideenbereich als zusätzlicher vertikaler Abstand zwischen den Zettelreihen verteilt wurde.
- Die Zettelreihen werden jetzt konsequent von oben nach unten kompakt angeordnet.
- Der gewünschte kleine Abstand zwischen den Papierstreifen bleibt unabhängig von der Fensterhöhe erhalten.

## v0.2.3

### 🎨 Zettelansicht

- Der vertikale Abstand zwischen den Ideenzetteln wurde deutlich reduziert.
- Die Papierstreifen wurden leicht kompakter gestaltet.
- Innenabstände und Abstand zwischen Ideentext und Kategorieangabe wurden angepasst.
- Dadurch können mehr Ideen gleichzeitig überblickt werden, ohne die papierartige Zettelkasten-Optik zu verlieren.

## v0.2.2

### 🔎 Suche

- Die große Suche durchsucht jetzt standardmäßig das gesamte Ideenarchiv – unabhängig davon, welche Kategorie gerade geöffnet ist.
- Über den Filter kann der Suchbereich bei Bedarf auf die aktuelle Kategorie oder auf die aktuelle Kategorie inklusive Unterkategorien begrenzt werden.
- Die Trefferanzeige zeigt jetzt zusätzlich an, in welchem Suchbereich gesucht wird.
- Die Exaktsuche verwendet dieselbe Suchbereichslogik und eignet sich damit zuverlässig zur Prüfung bereits vorhandener Textpassagen im gesamten Archiv.

## v0.2.1

### 🧹 Startbestand

- Die bisherigen Demo-Ideen wurden vollständig aus dem Ideenarchiv entfernt.
- Bereits durch frühere Versionen angelegte Demo-Einträge werden beim ersten Start der neuen Version automatisch bereinigt.
- Neue Archive starten jetzt ohne vorgegebene Ideenzettel und können direkt mit eigenen Inhalten aufgebaut werden.
- Die vorhandene Kategorienstruktur bleibt als Ausgangspunkt erhalten und kann weiterhin individuell angepasst werden.

## v0.2.0

### 📚 Kategorien

- Kategorien können jetzt direkt im Ideenarchiv angelegt und bearbeitet werden.
- Neue Unterkategorien können einer bestehenden übergeordneten Kategorie zugeordnet werden.
- Kategorien können gelöscht werden.
- Beim Löschen einer Kategorie werden vorhandene Ideen sinnvoll in die verbleibende Struktur übernommen.
- Eine Kategoriesuche erleichtert die Navigation in umfangreichen Kategorienbäumen.

### 🗃️ Ideen verwalten

- Ideen können jetzt komfortabel in eine andere Kategorie verschoben werden.
- Die Zielkategorie wird über eine Auswahl gewählt, statt über eine interne Kategorie-ID angegeben zu werden.

### ✍️ Massenerfassung

- Neuer Modus zur schnellen Erfassung vieler Ideen.
- Eine Zielkategorie kann einmalig ausgewählt werden.
- Anschließend können beliebig viele Ideen zeilenweise eingegeben werden.
- Jede Zeile wird beim Speichern automatisch als eigener Ideenzettel angelegt.
- Die Massenerfassung erleichtert insbesondere die Digitalisierung umfangreicher analoger Notizsammlungen.

### 💾 Backup & Import

- Das vollständige Ideenarchiv kann als JSON-Backup exportiert werden.
- Vorhandene JSON-Backups können wieder in die Anwendung importiert werden.
- Kategorien und Ideendaten werden gemeinsam gesichert.
- Damit steht erstmals eine grundlegende Sicherungs- und Wiederherstellungsfunktion für das lokale Archiv zur Verfügung.

### 🎨 Oberfläche

- Die bestehende papierbasierte Zettelkasten-Gestaltung wurde für die neuen Verwaltungsfunktionen beibehalten.
- Neue Funktionen wurden in die vorhandene Oberfläche integriert, ohne den kompakten Charakter des Ideenarchivs zu verändern.


## v0.1.1

### 📦 Projektstruktur

- Das Versionspaket wurde vereinfacht.
- Nach dem Entpacken befindet sich die `index.html` direkt im Hauptordner.
- Die Anwendung kann dadurch ohne zusätzliche verschachtelte Ordnerebene geöffnet werden.

---

## v0.1.0

### ✨ Erste Version

- Erste funktionsfähige Version des digitalen Ideenarchivs umgesetzt.
- Grundlegende lokale Browser-Anwendung für die Verwaltung großer Sammlungen von Storyideen erstellt.
- Die Oberfläche wurde als moderner digitaler Zettelkasten mit ausgeprägter Papierästhetik gestaltet.

### 🗃️ Ideenzettel

- Einzelne Ideen werden als eigenständige papierartige Zettel dargestellt.
- Ideen benötigen keinen zusätzlichen Titel.
- Kurze und längere Gedanken können direkt als eigentlicher Inhalt gespeichert werden.
- Kompakte mehrspaltige Darstellung ermöglicht das gleichzeitige Überfliegen vieler Ideen.
- Neue Ideen können schnell und ohne umfangreiche Eingabemaske angelegt werden.

### 📚 Kategorien

- Hierarchischer Kategorienbaum für die Organisation großer Ideensammlungen eingeführt.
- Kategorien und Unterkategorien können zur Navigation verwendet werden.
- Die Anzahl der enthaltenen Ideen wird direkt an den Kategorien angezeigt.
- Kategorienavigation und Zettelansicht sind miteinander verbunden.

### 🔎 Suche

- Volltextsuche für gespeicherte Ideen eingeführt.
- Separate Exaktsuche ermöglicht das gezielte Auffinden konkreter Textpassagen.
- Gefundene Textstellen werden innerhalb der Ergebnisse hervorgehoben.
- Die Suche ist auf die Arbeit mit mehreren tausend Ideen ausgelegt.

### 🎲 Zufall & Stöbern

- Zufallsauswahl für vorhandene Ideen eingeführt.
- Ideen können unabhängig von ihrer Kategorie erneut entdeckt werden.
- Das Archiv kann damit neben der gezielten Suche auch zur Inspiration genutzt werden.

### 📊 Archiv

- Gesamtzahl der gespeicherten Ideen wird prominent angezeigt.
- Das Wachstum der Sammlung wird innerhalb der Oberfläche sichtbar gemacht.
- Favoritenfunktion für wichtige Ideen ergänzt.
- Papierkorb für entfernte Einträge integriert.
- Sortiermöglichkeiten für die Ideenansicht ergänzt.
- Seitengrößen mit 24, 48 oder 96 Ideen pro Seite eingeführt.

### 🔄 Duplikate

- Beim Erfassen neuer Ideen kann auf mögliche bereits vorhandene Einträge hingewiesen werden.
- Die Funktion unterstützt insbesondere die spätere Digitalisierung umfangreicher analoger Notizsammlungen.

### 💾 Speicherung

- Lokale Speicherung der Ideendaten im Browser umgesetzt.
- IndexedDB bildet die Grundlage für die Verwaltung größerer Datenmengen.
- Für die Grundfunktionen sind weder Benutzerkonto noch eigener Server erforderlich.

### 🎨 Oberfläche

- Warme Schreibtisch- und Papierästhetik als grundlegende Designsprache eingeführt.
- Cremefarbene Flächen und dezente Pastelltöne bilden die Farbwelt.
- Papierstreifen ersetzen klassische quadratische Post-it-Karten.
- Analoge Details wie Klebestreifen, Büroklammern, Lochungen, Papierkanten und Register prägen die Oberfläche.
- Trotz der dekorativen Gestaltung wurde auf eine kompakte Darstellung für große Ideensammlungen geachtet.
