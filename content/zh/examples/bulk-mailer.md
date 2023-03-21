---
title: 批量邮件示例
---

在这个示例中，我们创建了一个小型的批量邮件发送程序，可以将相同的邮件发送给更多的收件人。 对于我们来说，重要的是直接在邮件中寻址收件人，因此我们将使用Go的`html/template`和`text/template`系统以及占位符。

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
// 准备不同的模板
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

// 通过SMTP发送邮件
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

At first, in [line 15](#hl-0-15), we define a new type for our users that we want to address. This is totally optional and is only done so we can easily work with a list of users and address them later on in our text template. How you handle this, is totally up to you and not mandatory for this to work.

In [line 28](#hl-0-28) thru [48](#hl-0-48) we set up a simple text and HTML template mail body with placeholders that can be used with Go's `html/template` and `text/template`.

Next we set up a list of users, we want to send our great bulk mailing to. [Line 52](#hl-0-52) uses the `User` type for this. With the preparation work done, we will start looping over all of our users in [line 70](#hl-0-70). For each user we create a new `*mail.Msg`.

For bulk mailings it is common that the `ENVELOPE FROM` and the `MAIL FROM` differ, so that bounce mails are sent to some system that can mark those bounces in the local system as bounced. Therefore we set both of those from addresses in [line 73](#hl-0-73) and [line 76](#hl-0-76). The lines [79](#hl-0-79) to [85](#hl-0-85) should be of no surprise to you, if you already used go-mail before.

One more interesting thing happens in [lines 86](#hl-0-86) thru [91](#hl-0-91) in which we use our prepared `html/template` and `text/template` templates and apply it to our mail message using `m.SetBodyHTMLTemplate` and `m.AddAlternativeTextTemplate`. We provide the whole user struct as data to that methods, so that `html/template` and `text/template` can take care of replacing placeholders in the mail body. Go-mail will take care of all the bells and whistles with the template handling for you. With our mail message now complete, we append it to our mail message slice in [line 93](#hl-0-93).

Finally we create a new [Client](/reference/client/) and send out all of our prepared messages in one go by providing the whole slice of messages to `Client.DialAndSend`.