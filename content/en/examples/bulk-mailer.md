---
title: Bulk Mailer Example
---

In this example we create a small bulk mailer for sending out the same mail to a bigger list of
recipients. It is important for us to address the recipient directly in the mail, therefore we will
also make use of Go's `text/template` system.

```go
package main

import (
	"fmt"
	"github.com/wneessen/go-mail"
	"log"
	"math/rand"
	"os"
	"text/template"
	"time"
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

const mailBodyTemplate = `Hi {{.Firstname}},

we are writing your to let you know that this week we have an amazing offer for you.
Using the coupon code "GOMAIL" you will get a 20% discount on all our products in our
online shop.

Check out our latest offer on https://acme.com and use your discount code today!

Your marketing team
  at ACME Inc.`

func main() {
	ul := []User{
		{"Toni", "Tester", "toni.tester@example.com"},
		{"Tina", "Tester", "tina.tester@example.com"},
		{"John", "Doe", "john.doe@example.com"},
	}
	tpl, err := template.New("template").Parse(mailBodyTemplate)
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
		if err := m.SetBodyTextTemplate(tpl, u); err != nil {
			log.Fatalf("failed to set template as text body: %s", err)
		}

		ms = append(ms, m)
	}

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

Let's take the example apart to look at some details...

At first, in [line 14](#hl-0-14), we define a new type for our users that we want to address. This is totally 
optional and is only done so we can easily work with a list of users and address them later on in our text 
template. How you handle this, is totally up to you and not mandatory for this to work.

In [line 26](#hl-0-26) we set up a simple text template mail body with placeholders that can be used with 
Go's `text/template`. If you like you can also do this in HTML and use `html/template` instead. In this case
you would use `m.SetBodyHTMLTemplate` instead of `m.SetBodyTextTemplate` in [line 66](#hl-0-66). Of course both
can be combined as well. For this we have the `Msg.AddAlternativeHTMLTemplate` and `Msg.AddAlternativeTextTemplate`
methods.
