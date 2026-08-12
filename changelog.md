# 📝 Ideenarchiv -- Changelog

Alle wichtigen Änderungen und Entwicklungsschritte des **Ideenarchivs**.

> Das Ideenarchiv wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt.
> Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

---

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
