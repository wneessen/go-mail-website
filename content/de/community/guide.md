---
title: Community-Leitfaden
---

Die go-mail-Community wächst und wenn Du dies liest, stehen die Chancen gut, dass Du auch mitmachen willst!

{{< toc >}}

## Ressourcen

### Verhaltenskodex

In unserer Community folgen wir dem [Verhaltenskodex](https://github.com/wneessen/go-mail/blob/main/CODE_OF_CONDUCT.md) und verlangen von jedem, der Teilnehmen möchte, sich ebenso zu verhalten.

### Support- und Ankündigungskanäle

* [Twitter](https://twitter.com/gomail_dev): Folge uns auf Twitter, um aktuelle Nachrichten über go-mail zu erhalten
* [go-mail Forum](https://github.com/wneessen/go-mail/discussions): Erhalte Ankündigungen und starte Diskussionen über go-mail.
* [Github Tickets](https://github.com/wneessen/go-mail/issues): Wenn Du einen Bug melden oder ein Feature vorschlagen möchtest, benutze bitte die Github "Issues" Funktion. Bitte beachte die Regeln, die in jedem Projektarchiv-Template angegeben sind.
* [Discord](https://discord.gg/zSUeBrsFPB): Ein Platz, wo go-mail Entwickler und Nutzer sich treffen können und in Echtzeit chatten können.

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

If you chose to test if someone's bug report is reproducible on your local system, then feel free to add a comment on the issue confirming this with the output of your test program.

To test PRs, choose a PR to test and check if the PR description has the testing scenarios listed. If not, please ask the person who opened the PR to provide that list. Once you have determined a valid test scenario, please report your findings on the PR.

If you ever need more clarity or help on testing, please ask a question in the [Github forum](https://github.com/wneessen/go-mail/discussions) or on [Discord](https://discord.gg/zSUeBrsFPB).

### Documentation

While we require proper GoDoc documenation comments in the code, this website is meant as more in-depth documenation of features and the project itself.

Since documenattion is hard and the website is still in an incomplete state, any contribution to this is greatly appreciated. Features without documentation are condidered "unfinished" to the project, it's as important as the code.

The website is built on Hugo using the Geekdocs theme. It's very simple and basically consists of markdown files. There are instructions on how to install the website on your local computer in the [website's repository](https://github.com/wneessen/go-mail-website).

### Translation

The default documents of the go-mail project are English documents. We use the "Crowdin" tool to translate documents in other languages and synchronize them to the website. You can [join our project](https://crwd.in/go-mail) and submit your translations to make contributions.

Currently the only supported 2nd language is German, but we are keen to add other languages as well. Please request them via a Github issue in the go-mail-website repository.

### Support

A great way to contribute to the project is to help others who are experiencing difficulty. This is normally reported as a issue or a message on the `#go-mail` [Discord channel](https://discord.gg/zSUeBrsFPB). Even just clarifying the issue can really help out. Sometimes, when an issue is discussed and gets resolved, we create a guide out of it to help others who face the same issues.

