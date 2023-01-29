---
title: Community-Leitfaden
---

Die go-mail-Community wächst und wenn Du dies liest, stehen die Chancen gut, dass Du auch mitmachen willst!

{{< toc >}}

## Ressourcen

### Verhaltenskodex

In unserer Community folgen wir dem [Verhaltenskodex](https://github.com/wneessen/go-mail/blob/main/CODE_OF_CONDUCT.md) und verlangen von jedem, der Teilnehmen möchte, sich ebenso zu verhalten.

### Support- und Ankündigungskanäle

* [Mastodon](https://s.pebcak.de/@go_mail/): Folge uns auf Mastodon, um aktuelle Nachrichten über go-mail zu erhalten
* [go-mail Forum](https://github.com/wneessen/go-mail/discussions): Erhalte Ankündigungen und starte Diskussionen über go-mail.
* [Github Tickets](https://github.com/wneessen/go-mail/issues): Wenn Du einen Bug melden oder ein Feature vorschlagen möchtest, benutze bitte die Github "Issues" Funktion. Bitte beachte die Regeln, die in jedem Projektarchiv-Template angegeben sind.
* [Discord](https://discord.gg/dbfQyC4s): Ein Ort, an dem sich Go-Mail-Entwickler und -Benutzer treffen und in Echtzeit chatten können.

## Mitwirken

go-mail ist ein Open-Source-Projekt, das von der Community betrieben wird. Wir begrüßen jeden, der mit uns an diesem Projekt mitwirkt. Diese Dokumentation richtet sich an alle, die sich mit dem Projekt und den Entwicklungsprozessen vertraut machen möchten.

* [Entwicklung neuer Funktionen](#developing-new-features)
* [Fehler beheben](#fixing-bugs)
* [Testen](#testing)
* [Dokumentation](#documentation)
* [Übersetzungen](#translation)
* [Unterstützen](#support)

<!-- https://crwd.in/go-mail //-->

### Entwicklung neuer Funktionen

Wir sind immer daran interessiert, Funktionen zu go-mail hinzuzufügen. Der Prozess zum Hinzufügen neuer Funktionen lautet wie folgt:

* Schau im [Issue-Bereich auf Github](https://github.com/wneessen/go-mail/issues) nach verfügbaren Issues mit dem Tag "TODO" oder "help wanted"
* Wenn kein offenes "TODO"/"Hilfe gesucht"-Thema gefunden wird oder die Funktion, die du im Sinn hast, nicht abgedeckt ist, mach bitte ein Issue für diese spezielle Funktion auf und warte auf das "OK" der Maintainers
* Überprüfe vor der Entwicklung, ob die Ausgabe die folgenden Informationen enthält:
  * Der Zweck der Erweiterung
  * Was für die Verbesserung nicht in Frage kommt
* Wenn das Issue diese Informationen nicht enthält, kannst du sie bei der Person, die das Issue eröffnet hat, anfordern. Manchmal werden Platzhalterausgaben erstellt und erfordern mehr Details
* Kommentiere das Thema und gib an, ob du die Funktion entwickeln möchtest
* Klone das Repository und erstelle einen Zweig mit dem Format `feature/<issue_number>_<issue_title>`
* Neue Funktionen erfordern oft eine Dokumentation, also stelle bitte sicher, dass du die Dokumentation als Teil der Änderungen auch hinzugefügt oder aktualisiert hast
* Bitte stelle sicher, dass dein Code die erforderliche Testabdeckung hat
* Sobald die Funktion zum Testen bereit ist, erstellst du einen PR-Entwurf. Bitte stelle sicher, dass in der PR-Beschreibung die Testszenarien und Testfälle mit Häkchen aufgeführt sind, damit andere wissen, was noch getestet werden muss
* Sobald alle Tests abgeschlossen sind, aktualisiere bitte den Status des PR von Entwurf und hinterlasse eine Nachricht

{{< hint type=important >}}
Alle PRs, die ohne eine entsprechende Ausgabe eröffnet werden, können abgelehnt werden.
{{< /hint >}}

### Fehler beheben

Das Verfahren zur Behebung von Fehlern ist wie folgt:

* Überprüfe die [Github Issues](https://github.com/wneessen/go-mail/issues) und wähle einen Fehler zum Beheben aus
* Überprüfe vor der Entwicklung, ob das Issue die folgenden Informationen enthält:
  * Der Umfang des Problems einschließlich der betroffenen Plattformen
  * Die Schritte zum Reproduzieren. Manchmal werden Bugs geöffnet, die keine go-mail Probleme sind, und der Berichterstatter muss anhand eines minimalen, reproduzierbaren Beispiels beweisen, dass es sich um ein go-mail Problem handelt
* Wenn das Problem diese Informationen nicht enthält, kannst du sie bei der Person, die das Problem eröffnet hat, anfordern
* Kommentiere das Issue und gib an, dass du eine Lösung entwickeln möchtest
* Klone das Repository und erstelle einen Branch mit dem Format `bugfix/<issue_number>_<issue_title>`
* Sobald die Korrektur zum Testen bereit ist, erstellst du einen PR-Entwurf. Bitte stelle sicher, dass in der PR-Beschreibung die Testszenarien und Testfälle mit Häkchen aufgeführt sind, damit andere wissen, was noch getestet werden muss
* Sobald alle Tests abgeschlossen sind, aktualisiere bitte den Status des PR von Entwurf und hinterlasse eine Nachricht.

{{< hint type=note >}}
Es hält dich nichts davon ab, ein Issue zu öffnen und selbst daran zu arbeiten, aber sei dir bitte bewusst, dass alle Fehlerbehebungen besprochen werden sollten, da der Ansatz unbeabsichtigte Nebenwirkungen haben kann.
{{< /hint >}}

{{< hint type=important >}}
Alle PRs, die ohne eine entsprechende Ausgabe eröffnet werden, können abgelehnt werden.
{{< /hint >}}


### Testen

Das Testen ist von entscheidender Bedeutung, um die Qualität des Projekts sicherzustellen. Es gibt ein paar Szenarien, in denen das Testen dem Projekt wirklich helfen kann:

* Testen, ob ein Fehler auf deinem lokalen System reproduzierbar ist
* PRs testen, um sicherzustellen, dass sie richtig funktionieren

Wenn du testen möchtest, ob die Fehlermeldung eines anderen Nutzers auf deinem System reproduzierbar ist, kannst du einen Kommentar zu der Meldung hinzufügen, der dies mit der Ausgabe deines Testprogramms bestätigt.

Um PRs zu testen, wählst du einen PR zum Testen aus und überprüfst, ob in der PR-Beschreibung die Testszenarien aufgeführt sind. Wenn nicht, bitte die Person, die den PR eröffnet hat, diese Liste zur Verfügung zu stellen. Wenn du ein gültiges Testszenario ermittelt hast, melde deine Ergebnisse bitte in der PR.

Wenn du jemals mehr Klarheit oder Hilfe beim Testen brauchst, stelle bitte eine Frage im [Github Forum](https://github.com/wneessen/go-mail/discussions) oder auf [Discord](https://discord.gg/dbfQyC4s).

### Dokumentation

Während wir eine ordentliche GoDoc-Dokumentation mit Kommentaren im Code verlangen, ist diese Website für eine ausführlichere Dokumentation der Funktionen und des Projekts selbst gedacht.

Da das Thema "Dokumentation" immer schwierig ist und die Website noch unvollständig ist, ist jeder Beitrag dazu sehr willkommen. Funktionen ohne Dokumentation gelten für das Projekt als "unvollendet", sie sind genauso wichtig wie der Code.

Die Website wurde mit Hugo und dem Geekdocs-Theme erstellt. Es ist sehr einfach und besteht im Wesentlichen aus Markdown-Dateien. Im [Repository der Website](https://github.com/wneessen/go-mail-website) findest du eine Anleitung, wie du die Website auf deinem lokalen Computer installieren kannst.

### Übersetzungen

Die Standarddokumente des go-mail-Projekts sind englische Dokumente. Wir verwenden das Tool "Crowdin", um Dokumente in andere Sprachen zu übersetzen und sie mit der Website zu synchronisieren. Du kannst [unserem Projekt beitreten](https://translations.go-mail.dev) und deine Übersetzungen einreichen, um einen Beitrag zu leisten.

Derzeit ist die einzige unterstützte zweite Sprache Deutsch, aber wir sind daran interessiert, auch andere Sprachen hinzuzufügen. Bitte beantrage sie über ein Github-Problem im go-mail-website Repository.

### Unterstützen

Eine gute Möglichkeit, zum Projekt beizutragen, ist es, anderen zu helfen, die Schwierigkeiten haben. Dies wird normalerweise als Problem oder als Nachricht auf dem `#go-mail` [Discord-Kanal](https://discord.gg/dbfQyC4s) gemeldet. Schon die Klärung des Problems kann sehr hilfreich sein. Manchmal, wenn ein Problem diskutiert und gelöst wird, erstellen wir daraus einen Leitfaden, um anderen zu helfen, die mit den gleichen Problemen konfrontiert sind.

