# 📝 Ideenarchiv -- Changelog

Alle wichtigen Änderungen und Entwicklungsschritte des **Ideenarchivs**.

> Das Ideenarchiv wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt.
> Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

---
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
