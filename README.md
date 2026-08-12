# 🗃️ Ideenarchiv

Ein persönlicher digitaler Zettelkasten für tausende Schreib- und Storyideen – schnell durchsuchbar, visuell organisiert und mit Fokus auf unkompliziertes Sammeln.

Das **Ideenarchiv** entsteht als browserbasierte JavaScript-Anwendung für die Verwaltung einer großen, über Jahre gewachsenen Sammlung von Ideen für Geschichten und Romane.

Statt klassischer Notizseiten werden einzelne Gedanken als digitale Papierzettel behandelt. Sie lassen sich hierarchisch einsortieren, durchsuchen, wiederentdecken und per Zufall neu hervorholen.

Die Anwendung ist der erste Baustein einer langfristig geplanten persönlichen Autoren-Suite für Ideenverwaltung, Romanplanung, Recherche, Manuskripte und Schreibstatistiken.

---

## ✨ Funktionen

### 🗃️ Digitaler Zettelkasten

- Jede Idee wird als eigenständiger Zettel gespeichert
- Keine Titel oder zusätzlichen Pflichtfelder notwendig
- Schnelle Erfassung kurzer und längerer Gedanken
- Papierartige Darstellung statt klassischer Datenbank- oder Tabellenansicht
- Kompakte mehrspaltige Darstellung auch für große Ideensammlungen

### 📚 Kategorien & Unterkategorien

- Hierarchische Organisation der Ideen
- Kategorien und Unterkategorien können verschachtelt werden
- Direkte Anzeige der jeweiligen Ideenanzahl
- Navigation durch den Kategorienbaum
- Ideen bleiben auch bei umfangreichen Archiven strukturiert zugänglich

### 🔎 Suche

- Volltextsuche über das Ideenarchiv
- Separate Exaktsuche nach konkreten Textpassagen
- Suchtreffer werden direkt im Text hervorgehoben
- Suche innerhalb großer Ideensammlungen
- Kombination aus Suche und Kategorienavigation

Die Exaktsuche ist insbesondere dafür gedacht, beim Digitalisieren älterer Notizbücher schnell überprüfen zu können, ob eine bestimmte Idee bereits im Archiv vorhanden ist.

### 🎲 Ideen wiederentdecken

- Zufällige Idee aus dem Archiv anzeigen
- Bestehende Gedanken können unabhängig von ihrer ursprünglichen Ablage wiederentdeckt werden
- Stöberfunktion als Alternative zur gezielten Suche

### 📊 Wachstum

- Gesamtzahl aller gespeicherten Ideen
- Sichtbare Entwicklung des Archivs
- Neue Einträge erhöhen den Ideenzähler unmittelbar
- Grundlage für umfangreichere Statistiken in späteren Versionen

### ⭐ Organisation

- Ideen können favorisiert werden
- Papierkorb für entfernte Einträge
- Sortiermöglichkeiten
- Auswahl verschiedener Seitengrößen
- Hinweise auf mögliche doppelte Ideen

### 💾 Lokale Speicherung

- Speicherung direkt im Browser
- Keine Registrierung notwendig
- Kein externer Server für die Grundfunktionen erforderlich
- IndexedDB als Grundlage für größere Datenmengen

---

## 🎨 Design

Das Ideenarchiv verbindet eine moderne Benutzeroberfläche mit der Optik eines analogen Zettelkastens.

Statt typischer SaaS-Karten oder Tabellen stehen Papier, Karteikarten und ein warmer Schreibtischcharakter im Mittelpunkt.

Zum visuellen Konzept gehören unter anderem:

- warme Holz- und Papieroberflächen
- cremefarbene Grundflächen
- dezente Pastelltöne
- schmale Papierstreifen für einzelne Ideen
- unterschiedliche Papierarten
- Büroklammern, Klebestreifen, Lochungen und Papierkanten als Details
- Register und Kategorien im Stil eines echten Archivs
- hohe Informationsdichte trotz analoger Gestaltung

Die Gestaltung ist dabei nicht nur dekorativ: Das sichtbare Wachstum des Archivs und das Wiederentdecken alter Ideen sollen zum Sammeln und Schreiben motivieren.

---

## 🧠 Hintergrund

Die Anwendung entsteht aus einem konkreten eigenen Bedarf.

Die bisherige Ideensammlung umfasst bereits mehrere tausend einzelne Storyideen und wurde teilweise in **Obsidian** digitalisiert. Weitere Ideen befinden sich noch in analogen Notizbüchern.

Klassische Notizprogramme bilden diese Art der Sammlung nur bedingt ab: Eine einzelne Idee ist häufig kein vollständiges Dokument mit Titel und Metadaten, sondern lediglich ein Satz, Gedanke oder Storybaustein.

Das Ideenarchiv behandelt deshalb bewusst:

> **eine Idee als einen Zettel.**

Die Anwendung soll langfristig auch sehr große Archive mit zehntausenden Einträgen übersichtlich und schnell durchsuchbar halten.

---

## 🚀 Geplante Weiterentwicklung

Das Ideenarchiv bildet den ersten Baustein einer größeren persönlichen Schreibumgebung.

Geplant sind unter anderem:

- Import bestehender Obsidian-/Markdown-Sammlungen
- komfortable Massenerfassung für die Digitalisierung analoger Notizbücher
- erweiterte Suche und Filter
- umfangreichere Archivstatistiken
- intelligente Erkennung möglicher Duplikate
- zusätzliche Zufalls- und Kombinationsfunktionen
- Romanideen als eigener Inhaltstyp
- Namensarchive
- Recherche- und Inspirationsbereiche
- Bilder, Musik und Videos
- Moodboards
- konkrete Romanprojekte
- Figuren-, Orts- und Weltplanung
- Kapitel- und Szenenplanung
- integrierter Manuskriptbereich
- Schreibsessions und Wortzahlenerfassung
- ausführliche Schreibstatistiken

Die einzelnen Bereiche sollen langfristig miteinander verknüpft werden, ohne die schnelle und unkomplizierte Arbeitsweise des Ideenarchivs zu verlieren.

---

## 🛠️ Technik

- HTML
- CSS
- JavaScript
- IndexedDB
- lokale Browser-Anwendung

Die Anwendung wird zunächst vollständig lokal entwickelt und benötigt für ihre Grundfunktionen weder Benutzerkonto noch eigenen Server.

---

## 📌 Status

**Aktuelle Version:** v0.1.1

Das Projekt befindet sich in aktiver Entwicklung.

Die erste Entwicklungsphase konzentriert sich auf den Kern des Ideenarchivs: schnelles Erfassen, zuverlässiges Wiederfinden, hierarchische Organisation und eine visuelle Zettelkasten-Oberfläche, die auch bei sehr großen Sammlungen übersichtlich bleibt.

---

## 📄 Lizenz

Dieses Projekt ist ein persönliches Softwareprojekt.

Eine Nutzung, Weitergabe oder Veröffentlichung des Quellcodes ist ohne ausdrückliche Erlaubnis nicht vorgesehen.
