---
title: Einfaches Mailer-Beispiel
---

Dieses Beispiel ist der simpelste Code, der erforderlich ist, um eine E-Mail mit go-mail erfolgreich zu versenden.

```go
package main

import (
	"log"
	"os"

	"github.com/wneessen/go-mail"
)

func main() {
    msg := mail.NewMsg()
    if err := msg.From("toni@tester.com"); err != nil {
        log.Fatalf("failed to set FROM address: %s", err)
    }
    if err := msg.To("tina@recipient.org"); err != nil {
        log.Fatalf("failed to set TO address: %s", err)
    }
    msg.Subject("This is my first test mail with go-mail!")
    msg.SetBodyString(mail.TypeTextPlain, "This will be the content of the mail.") 
    
    // Deliver the mails via SMTP 
    c, err := mail.NewClient("smtp.example.com", 
        mail.WithSMTPAuth(mail.SMTPAuthPlain), mail.WithTLSPortPolicy(mail.TLSMandatory), 
        mail.WithUsername(os.Getenv("SMTP_USER")), mail.WithPassword(os.Getenv("SMTP_PASS")), 
    )
    if err != nil {
        log.Fatalf("failed to create new mail delivery client: %s", err)
    }
    if err := c.DialAndSend(msg); err != nil {
        log.Fatalf("failed to deliver mail: %s", err)
    }
    log.Printf("Test mail successfully delivered.")
}
```

In diesem Beispiel wird eine einfache Test-Mail von `toni@tester.com` an `tina@recipient.org` gesendet.

Zunächst erstellen wir in [Zeile 11](#hl-0-11) eine neue `Msg`. Der Typ `Msg` enthält alles, was für
für deine Mailnachricht. Stell dir das wie eine neue Mail in deinem Mail-Benutzer-Agent vor. Der Typ `Msg`
stellt dir alle Methoden zur Verfügung, die du zur Vorbereitung und Formatierung deiner Mailnachricht benötigst.

In [Zeile 12](#hl-0-12) bis [14](#hl-0-14) legen wir die Absenderadresse fest. In diesem Fall ist es `toni@tester.com`.
Da go-mail unter der Haube eine Menge Validierungen durchführt, stellt es sicher, dass die angegebene Mailadresse
gültig ist. Deshalb prüfen wir den zurückgegebenen Fehler. In [Zeile 15](#hl-0-15) bis [17](#hl-0-17) machen wir das Gleiche
dasselbe für die Empfängeradresse. Die E-Mail wird an `tina@recipient.org` gesendet.

Als Nächstes legen wir in [Zeile 18](#hl-0-18) den Betreff für unsere E-Mail-Nachricht fest. Gefolgt vom Setzen einer einfachen Zeichenfolge
als Nachrichtentext in [Zeile 19](#hl-0-19). Das erste Argument für `Msg.SetBodyString()` ist ein MIME Content Type.
In diesem Fall verwenden wir `mail.TypeTextPlain` für den Mailbody - also einen einfachen `plain/text` Mailbody.

Jetzt, wo unsere einfache E-Mail-Nachricht für den Versand vorbereitet ist, können wir einen "Client" erstellen. Der `Client` in go-mail
verwaltet die Verbindung zu einem Mailserver und alles, was damit zusammenhängt. In [Zeile 22](#hl-0-22) bis
[25](#hl-0-25) initialisieren wir einen neuen `Client`-Typ und geben ihm einige Optionen. Das erste Argument ist der
Mailserver, mit dem wir uns verbinden wollen. In unserem Beispiel ist es `smtp.example.com`. Then we tell the `Client`
that we need to perform SMTP Auth via the `PLAIN` auth method. The `mail.WithSMTPAuth(mail.SMTPAuthPlain)`
option handles this. With the `mail.WithTLSPortPolicy(mail.TLSMandatory)` option, we tell the `Client`
that we require a TLS connection for sending our mail. We set it to mandatory mode, so that the `Client`
will stop processing if it cannot accomplish a TLS secured connection. With the `mail.WithUsername()`
and `mail.WithPassword()` options, we let the `Client` know what username and password to use for the
SMTP auth. [Lines 26-28](#hl-0-26) check if the `Client` invocation worked without errors.

Finally, in [line 29-32](#hl-0-29) we send out our mail message using our mail client with the
`Client.DialAndSend()` method. We print out a brief message on success or throw an error in case
the delivery failed.
