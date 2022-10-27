---
title: Beispiel für Massenmailer
---

In diesem Beispiel erstellen wir einen kleinen Massenversender, um die gleiche Mail an eine größere Liste von Empfängern zu versenden. Für uns ist es wichtig, den Empfänger in der Mail direkt anzusprechen. Deshalb werden wir das `html/template` und `text/template` System von Go zusammen mit Platzhaltern verwenden.

```go
package main

import (
    "fmt"
    ht "html/template"
    "log"
    "math/rand"
    "os"
    tt "text/template"
    "time"

    "github.com/wneessen/go-mail"
)

// User is a simple type allowing us to set a firstname, lastname and mail address
type User struct {
    Firstname string
    Lastname  string
    EmailAddr string
}

// Our sender information that will be used in the FROM address field
const (
    senderName = "ACME Inc."
    senderAddr = "noreply@acme.com"
)

const (
    textBodyTemplate = `Hi {{.Firstname}},

we are writing your to let you know that this week we have an amazing offer for you.
Using the coupon code "GOMAIL" you will get a 20% discount on all our products in our
online shop.

Check out our latest offer on https://acme.com and use your discount code today!

Your marketing team
  at ACME Inc.`

    htmlBodyTemplate = `<p>Hi {{.Firstname}},</p>
<p>we are writing your to let you know that this week we have an amazing offer for you.
Using the coupon code "<strong>GOMAIL</strong>" you will get a 20% discount on all 
our products in our online shop.</p>
<p>Check out our latest offer on <a href="https://acme.com" target="_blank">https://acme.com</a>
and use your discount code today!</p>
<p>Your marketing team<br />
&nbsp;&nbsp;at ACME Inc.</p>`
)

func main() {
    // Define a list of users we want to mail to
    ul := []User{
        {"Toni", "Tester", "toni.tester@example.com"},
        {"Tina", "Tester", "tina.tester@example.com"},
        {"John", "Doe", "john.doe@example.com"},
    }

    // Prepare the different templates
    ttpl, err := tt.New("texttpl").Parse(textBodyTemplate)
    if err != nil {
        log.Fatalf("failed to parse text template: %s", err)
    }
    htpl, err := ht.New("htmltpl").Parse(htmlBodyTemplate)
    if err != nil {
        log.Fatalf("failed to parse text template: %s", err)
    }

    var ms []*mail.Msg
    r := rand.New(rand.NewSource(time.Now().UnixNano()))
    for _, u := range ul {
        rn := r.Int31()
        m := mail.NewMsg()
        if err := m.EnvelopeFrom(fmt.Sprintf("noreply+%d@acme.com", rn)); err != nil {
            log.Fatalf("failed to set ENVELOPE FROM address: %s", err)
        }
        if err := m.FromFormat(senderName, senderAddr); err != nil {
            log.Fatalf("failed to set formatted FROM address: %s", err)
        }
        if err := m.AddToFormat(fmt.Sprintf("%s %s", u.Firstname, u.Lastname), u.EmailAddr); err != nil {
            log.Fatalf("failed to set formatted TO address: %s", err)
        }
        m.SetMessageID()
        m.SetDate()
        m.SetBulk()
        m.Subject(fmt.Sprintf("%s, we have a great offer for you!", u.Firstname))
        if err := m.SetBodyHTMLTemplate(htpl, u); err != nil {
            log.Fatalf("failed to set HTML template as HTML body: %s", err)
        }
        if err := m.AddAlternativeTextTemplate(ttpl, u); err != nil {
            log.Fatalf("failed to set text template as alternative body: %s", err)
        }

        ms = append(ms, m)
    }

    // Deliver the mails via SMTP
    c, err := mail.NewClient("smtp.example.com",
        mail.WithSMTPAuth(mail.SMTPAuthPlain), mail.WithTLSPolicy(mail.TLSMandatory),
        mail.WithUsername(os.Getenv("SMTP_USER")), mail.WithPassword(os.Getenv("SMTP_PASS")),
    )
    if err := c.DialAndSend(ms...); err != nil {
        log.Fatalf("failed to deliver mail: %s", err)
    }
    log.Printf("Bulk mailing successfully delivered.")
}
```

Nehmen wir das Beispiel mal auseinander, um uns ein paar Details anzuschauen...

Zuerst, in [Zeile 15](#hl-0-15), definieren wir einen neuen Typ für unsere Nutzer, die wir ansprechen wollen. Das ist völlig optional und wird nur gemacht, damit wir mit einer Liste von Benutzern arbeiten und sie später in unserer Textvorlage ansprechen können. Wie du das handhabst, liegt ganz bei dir und ist nicht zwingend notwendig, damit es funktioniert.

In [Zeile 28](#hl-0-28) bis [48](#hl-0-48) haben wir einen einfachen Text- und HTML-Template-Mailkörper mit Platzhaltern eingerichtet, der mit Go's `html/template` und `text/template` verwendet werden kann.

Als Nächstes erstellen wir eine Liste von Nutzern, an die wir unser tolles Massenmailing schicken wollen. [Zeile 52](#hl-0-52) verwendet dafür den Typ `User`. Nachdem wir die Vorbereitungen getroffen haben, beginnen wir in [Zeile 70](#hl-0-70) mit einer Schleife über alle unsere Benutzer. Für jeden Benutzer erstellen wir eine neue `*mail.Msg`.

Bei Massenmails ist es üblich, dass die `ENVELOPE FROM` und die `MAIL FROM` unterschiedlich sind, so dass Bounce-Mails an ein System gesendet werden, das diese Bounces im lokalen System als gebounced markieren kann. Deshalb setzen wir diese beiden von Adressaten in [Zeile 73](#hl-0-73) und [Zeile 76](#hl-0-76). Die Zeilen [79](#hl-0-79) bis [85](#hl-0-85) sollten dich nicht überraschen, wenn du go-mail schon einmal benutzt hast.

Eine weitere wichtige Sache passiert in [Zeilen 86](#hl-0-86) bis [91](#hl-0-91), in denen wir unsere vorbereiteten `html/template` und `text/template` Vorlagen verwenden und sie mit `m.SetBodyHTMLTemplate` und `m.AddAlternativeTextTemplate` auf unsere E-Mail-Nachricht anwenden. Wir stellen dieser Methode die gesamte Benutzerstruktur als Daten zur Verfügung, so dass `html/template` und `text/template` sich um das Ersetzen der Platzhalter im Mailkörper kümmern können. Go-mail kümmert sich um den ganzen Schnickschnack mit der Vorlagenbearbeitung für dich. Da unsere Mailnachricht nun vollständig ist, fügen wir sie in [Zeile 93](#hl-0-93) an unsere Mailnachrichtenscheibe an.

Zum Schluss erstellen wir einen neuen [Client](/reference/client/) und senden alle unsere vorbereiteten Nachrichten in einem Rutsch, indem wir die gesamte Nachrichtenscheibe an `Client.DialAndSend` übergeben.