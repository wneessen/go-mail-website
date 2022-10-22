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

Das erste Argument für `SetBodyString()` ist ein Inhaltstyp, den wir angeben müssen. In our example the `mail.TypeTextPlain` basically represents a `text/plain` content type - meaning a plain text mail body.

### Sending the mail

Now that we have our mail message ready to go, let's bring it on the way and send it out. For this we'll use the `Client`, which handles the SMTP transmission.

```go
c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain), 
    mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
if err != nil {
    log.Fatalf("failed to create mail client: %s", err)
}
```

In this example we connect to the mail server with the hostname `smtp.example.com` and provide the `Client` with a couple of options like the port we want to connect to, the fact that we want to use `SMTP PLAIN` for authentication and the username and password.

Finally we tell the client to deliver the mail.

```go
if err := c.DialAndSend(m); err != nil {
    log.Fatalf("failed to send mail: %s", err)
}
```

The `DialAndSend()` method takes care of establishing the connection and sending out the mail. You have the option to call them separately as well, but we won't need this for the quick example.

## Conclusion

That was quite simple, wasn't it? You successfully prepared a mail message and delivered it to the recipient via a 3rd party mail server. go-mail of course can do much more. Check out the in-depth documentation for all the features.

## Full example code

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
    m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!")
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