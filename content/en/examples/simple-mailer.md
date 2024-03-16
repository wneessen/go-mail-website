---
title: Simple Mailer Example
---

This example is the most simple piece of code that is required to successfully send a mail with
go-mail.

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

This example will send a very basic test mail from `toni@tester.com` to `tina@recipient.org`.

First, in [line 11](#hl-0-11), we create a new `Msg`. The `Msg` type holds everything that is required
for your mail message. Think of it like a new mail within your mail user agent. The `Msg` type 
provides you with all of the methods that are required to prepare and format your mail message. 

In [line 12](#hl-0-12) thru [14](#hl-0-14) we set the sender address. In this case it's `toni@tester.com`.
Since go-mail is doing a lot of validation under the hood, it will make sure that the provided mail address
is valid. Therefore we check the returned error. In [line 15](#hl-0-15) thru [17](#hl-0-17) we do the 
same for the recipient address. The mail will be sent to `tina@recipient.org`.

Next, in [line 18](#hl-0-18) we set our subject for our mail message. Followed by setting a simple string
as message body in [line 19](#hl-0-19). The first argument for `Msg.SetBodyString()` is a MIME content type. 
In this case we use `mail.TypeTextPlain` for the mail body - so a simple `plain/text` mail body.

Now that our simple mail message is prepared for delivery, we can create a `Client`. The `Client` in go-mail
handles the connection to a mail server and everything related to it. In [line 22](#hl-0-22) thru 
[25](#hl-0-25) we initialize a new `Client` type and give it some options. The first argument is the
mail server we want to connect to. In our example it's `smtp.example.com`. Then we tell the `Client`
that we need to perform SMTP Auth via the `PLAIN` auth method. The `mail.WithSMTPAuth(mail.SMTPAuthPlain)`
option handles this. With the `mail.WithTLSPortPolicy(mail.TLSMandatory)` option, we tell the `Client`
that we require a TLS connection for sending our mail. We set it to mandatory mode, so that the `Client`
will stop processing if it cannot accomplish a TLS secured connection. With the `mail.WithUsername()`
and `mail.WithPassword()` options, we let the `Client` know what username and password to use for the
SMTP auth. [Lines 26-28](#hl-0-26) check if the `Client` invocation worked without errors.

Finally, in [line 29-32](#hl-0-29) we send out our mail message using our mail client with the 
`Client.DialAndSend()` method. We print out a brief message on success or throw an error in case 
the delivery failed.