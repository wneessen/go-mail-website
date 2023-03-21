---
title: 批量邮件示例
---

在这个示例中，我们创建了一个小型的批量邮件发送程序，可以将相同的邮件发送给更多的收件人。对于我们来说，重要的是直接在邮件中寻址收件人，因此我们将使用Go的`html/template`和`text/template`系统以及占位符。

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

// User是一个简单的类型，允许我们设置名字，姓氏和邮件地址
type User struct {
	Firstname string
	Lastname  string
	EmailAddr string
}

// 我们的发件人信息将用于FROM地址字段
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
	// 定义我们要发送邮件的用户列表
	ul := []User{
		{"Toni", "Tester", "toni.tester@example.com"},
		{"Tina", "Tester", "tina.tester@example.com"},
		{"John", "Doe", "john.doe@example.com"},
	}

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
