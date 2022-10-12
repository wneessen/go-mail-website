---
title: Erste Schritte
weight: -20
---

Dieses kurze Tutorial zeigt Dir, wie Du go-mail installierst und Deine erste Mail versendest.

<!--more-->

{{< toc >}}

## Anforderungen

go-mail erfordert eine funktionierende Go-Installation (Version 1.16+). Lade Go von der
[Go-Download-Seite](https://go.dev/dl/) herunter.

## Installation

go-mail kann mit Hilfe des Go-Modul-Installationsmechanismus über den Befehl `go install` installiert werden.

Um die neueste Version von go-mail zu installieren, geben Sie einfach den folgenden Befehl ein:

```Shell
$ go install github.com/wneessen/go-mail@latest
```

## Versende Deine erste Mail

go-mail besteht aus zwei Hauptkomponenten. Die `Msg`, die die Mail-Nachricht darstellt und der `Client`, der sich 
um die den Mailversand über einen SMTP-Dienst übernimmt.

### Eine neue Nachricht erstellen

Zuerst erstellen wir eine neue `Msg` mit der Methode `NewMsg()` und weisen eine Absenderadresse sowie eine 
Empfängeradresse zu.

{{< highlight Go "linenos=table" >}}
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
{{< /highlight >}}

In diesem kleinen Codeschnipsel importieren wir zuallererst go-mail in unser Projekt. Siehe die 
Anweisung `import` in Zeile 4. Als nächstes erstellen wir eine neue Nachricht in Zeile 9. In 
den Zeilen 10 und 13 werden die Absender- und Empfängeradressen festgelegt. go-mail stellt sicher, dass Du
gültige Mailadressen angeben hast, daher geben wir einen `error` zurück. Auf diese Weise können wir
sicherstellen, dass die angegebene Adressen von go-mail akzeptiert wird und später keine Probleme verursacht.

Als Nächstes wollen wir eine Betreffzeile für unsere Nachricht festlegen und den Mailtext mit Inhalt füllen.
{{< highlight Go "linenos=table" >}}
func main() {
    [...]
    m.Subject("This is my first mail with go-mail!")
    m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!")
}
{{< /highlight >}}

Als erstes Arguemtn für `SetBodyString()` müssen wir einen `ContentType` angeben. In unserem Beispiel nutzen wir
`Mail.TypeTextPlain`, was ganz einfach einen "text/plain"-Inhaltstyp repräsentiert.

### Versand der E-Mail

Nun, da wir unsere E-Mail-Nachricht fertig haben, können wir sie auf den Weg bringen und verschicken. 
Hierfür werden wir den `Client` verwenden, der die SMTP-Übertragung abwickelt.

{{< highlight Go "linenos=table" >}}
func main() {
    [...]
    c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain),
        mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
    if err != nil {
        log.Fatalf("failed to create mail client: %s", err)
    }
}
{{< /highlight >}}

In diesem Beispiel verbinden wir uns mit dem Mailserver mit dem Hostnamen `smtp.example.com` und geben dem
`Client` ein paar Optionen mit, wie den Port mit dem wir uns verbinden wollen, der Tatsache, dass wir
`SMTP PLAIN` für die Authentifizierung benutzen wollen sowie den Benutzernamen und das Passwort.

Schlußendlich weisen wir den Client an, die E-Mail zuzustellen.

{{< highlight Go "linenos=table" >}}
func main() {
    [...]
    if err := c.DialAndSend(m); err != nil {
        log.Fatalf("failed to send mail: %s", err)
    }
}
{{< /highlight >}}

Die Methode `DialAndSend()` kümmert sich um den Aufbau der Verbindung und den Versand der Mail. Du hast
die Möglichkeit, diese Methoden auch separat aufzurufen, aber für dieses kurze Beispiel ist das nicht
nötig.

## Fazit

Das war doch ganz einfach, oder? Du hast erfolgreich eine E-Mail-Nachricht vorbereitet und über einen 
fremden Mailserver an den Empfänger über einen 3rd-Party-Mailserver zugestellt. go-mail kann natürlich noch viel 
mehr. Sieh Dir dafür einfach die ausführliche Dokumentation an.

## Kompletter Beispielcode

{{< highlight Go "linenos=table" >}}
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
{{< /highlight >}}
