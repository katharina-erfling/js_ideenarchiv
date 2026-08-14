# 📝 Ideenarchiv -- Changelog

Alle wichtigen Änderungen und Entwicklungsschritte des **Ideenarchivs**.

> Das Ideenarchiv wird iterativ anhand einer realen, umfangreichen Sammlung von Storyideen weiterentwickelt.
> Der Changelog dokumentiert Funktionen, Verbesserungen und Fehlerbehebungen, ohne interne Implementierungsdetails offenzulegen.

---
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
