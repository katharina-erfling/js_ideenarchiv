# 📝 Ideenarchiv -- Changelog

Alle wichtigen Änderungen und Entwicklungsschritte des **Ideenarchivs**.

> Das Ideenarchiv wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt.
> Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

---
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
