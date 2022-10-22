---
title: Einführung
weight: -20
---

Diese kurze Anleitung zeigt dir, wie du go-mail von der Installation bis zum Versand deiner ersten Mail nutzen kannst.

<!--more-->

{{< toc >}}

## Voraussetzungen

go-mail erfordert eine funktionierende Go-Installation (Version 1.16+). Lade Go von der [Go Downloads Seite](https://go.dev/dl/).

## Installation

go-mail kann über den Go-Modul-Installationsmechanismus mit dem Befehl `go get` installiert werden.

Um die neueste Version von go-mail zu installieren, gehst du in deinen Projektordner und importierst das Modul einfach mit dem folgenden Befehl:

```shell
$ go get github.com/wneessen/go-mail
```

## Deine erste Mail verschicken

go-mail besteht aus zwei Hauptkomponenten. Die `Msg`, die die Mail-Nachricht darstellt und der `Client`, der sich um die Mail-Zustellung über einen SMTP-Dienst kümmert.

### Eine neue Nachricht erstellen

Zuerst erstellen wir eine neue `Msg` mit der Methode `NewMsg()` und weisen eine Absender- und eine Empfängeradresse zu.

```go
package main

import (
    "github.com/wneessen/go-mail"
    "log"
)

func main() {
    m := mail.NewMsg()
    if err := m.From("toni.sender@example.com"); err != nil {
        log.Fatalf("failed to set From address: %s", err)
    }
    if err := m.To("tina.recipient@example.com"); err != nil {
        log.Fatalf("failed to set To address: %s", err)
    }
}
```

In diesem kleinen Codeschnipsel importieren wir zuallererst go-mail in unser Projekt. Siehe die `Import`-Anweisung in [Zeile 4](#hl-1-4). Als nächstes erstellen wir eine neue Nachricht in [Zeile 9](#hl-1-9). In den Zeilen [10](#hl-1-10) und [13](#hl-1-13) werden die Absender- und Empfängeradressen festgelegt. Da go-mail sicherstellt, dass du gültige Mailadressen angibst, geben wir einen `error` zurück. Auf diese Weise können wir sicherstellen, dass die angegebene Adresse von go-mail akzeptiert wird und später keine Probleme verursacht.

Als Nächstes wollen wir eine Betreffzeile für unsere Nachricht festlegen und den Mailtext mit einem Inhalt füllen.

```go
m.Subject("Das ist meine erste Mail mit go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Gefällt dir diese Mail? Mir auf jeden Fall!")
```

Das erste Argument für `SetBodyString()` ist ein Inhaltstyp, den wir angeben müssen. In unserem Beispiel repräsentiert der `mail.TypeTextPlain` einen `text/plain` Inhaltstyp - also einen reinen Textkörper.

### Versenden der Mail

Jetzt, wo wir unsere E-Mail-Nachricht versandfertig haben, können wir sie auf den Weg bringen und verschicken. Hierfür verwenden wir den `Client`, der die SMTP-Übertragung abwickelt.

```go
c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain), 
    mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
if err != nil {
    log.Fatalf("failed to create mail client: %s", err)
}
```

In diesem Beispiel verbinden wir uns mit dem Mailserver hinter dem Hostnamen `smtp.example.com` und geben dem `Client` ein paar Optionen wie den Port, mit dem wir uns verbinden wollen, die Tatsache, dass wir `SMTP PLAIN` für die Authentifizierung verwenden wollen und den Benutzernamen und das Passwort, mit.

Abschließend weisen wir den Client an, die Mail zuzustellen.

```go
if err := c.DialAndSend(m); err != nil {
    log.Fatalf("failed to send mail: %s", err)
}
```

Die Methode `DialAndSend()` kümmert sich um den Aufbau der Verbindung und den Versand der Mail. Du kannst sie auch separat aufrufen, aber das benötigen wir für dieses kurze Beispiel nicht.

## Zusammenfassung

Das war doch ganz einfach, oder? Du hast erfolgreich eine E-Mail-Nachricht vorbereitet und sie dem Empfänger über einen Mailserver eines Drittanbieters zugestellt. go-mail kann natürlich noch viel mehr. In der ausführlichen Dokumentation findest du alle Funktionen.

## Vollständiger Beispielcode

```go
package main

import (
    "github.com/wneessen/go-mail"
    "log"
)

func main() {
    m := mail.NewMsg()
    if err := m.From("toni.sender@example.com"); err != nil {
        log.Fatalf("failed to set From address: %s", err)
    }
    if err := m.To("tina.recipient@example.com"); err != nil {
        log.Fatalf("failed to set To address: %s", err)
    }
    m.Subject("This is my first mail with go-mail!")
    m.SetBodyString(mail.TypeTextPlain, "Gefällt dir diese Mail? Mir auf jeden Fall!")
    c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain),
        mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
    if err != nil {
        log.Fatalf("failed to create mail client: %s", err)
    }
    if err := c.DialAndSend(m); err != nil {
        log.Fatalf("failed to send mail: %s", err)
    }
}
```