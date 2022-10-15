---
title: Introduction
weight: -20
---

This short tutorial shows you how to get up and running with go-mail from installation to sending your first mail.

<!--more-->

{{< toc >}}

## Requirements

go-mail requires a working Go installation (Version 1.16+). Download Go from the [Go Downloads Page](https://go.dev/dl/).

## Installation

go-mail can be installed using the Go module installation mechanism via the `go get` command.

To install the latest version of go-mail, enter your project folder and simply import the module by issuing the following command:

```Shell
$ go get github.com/wneessen/go-mail
```

## Sending your first mail

go-mail consists of two main components. The `Msg` which represents the mail message and the `Client` which takes care of the mail delivery via a SMTP service.

### Create a new message

First let's create a new `Msg` using the `NewMsg()` method and assign a sender address as well as a recipient address.

{{< highlight Go "linenos=table" >}}
package main

import ( "github.com/wneessen/go-mail" "log" )

func main() { m := mail.NewMsg() if err := m.From("toni.sender@example.com"); err != nil { log.Fatalf("failed to set From address: %s", err) } if err := m.To("tina.recipient@example.com"); err != nil { log.Fatalf("failed to set To address: %s", err) } }
{{< /highlight >}}

In this little code snippet, first and foremost we import go-mail into our project. See the `import` statement in line 4. Next we create a new message in line 9. Lines 10 and 13 set the sender and recipient addresses. Since go-mail makes sure that you are providing valid mail addresses, we return an `error`. This way we can make sure that the provided address is accepted by go-mail and will not cause problems later on.

Next we want to set a subject line for our message and fill the mail body with some content.
{{< highlight Go "linenos=table" >}}
func main() { [...] m.Subject("This is my first mail with go-mail!") m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") }
{{< /highlight >}}

The first argument for `SetBodyString()` is a content type we need to provide. In our example the `mail.TypeTextPlain` basically represents a `text/plain` content time - meaning a plain text mail body.

### Sending the mail

Now that we have our mail message ready to go, let's bring it on the way and send it out. For this we'll use the `Client`, which handles the SMTP transmission.

{{< highlight Go "linenos=table" >}}
func main() { [...] c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain), mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass")) if err != nil { log.Fatalf("failed to create mail client: %s", err) } }
{{< /highlight >}}

In this example we connect to the mail server with the hostname `smtp.example.com` and provide the `Client` with a couple of options like the port we want to connect to, the fact that we want to use `SMTP PLAIN` for authentication and the username and password.

Finally we tell the client to deliver the mail.

{{< highlight Go "linenos=table" >}}
func main() { [...] if err := c.DialAndSend(m); err != nil { log.Fatalf("failed to send mail: %s", err) } }
{{< /highlight >}}

The `DialAndSend()` method takes care of establishing the connection and sending out the mail. You have the option to call them separately as well, but we won't need this for the quick example.

## Conclusion

That was quite simple, wasn't it? You successfully prepared a mail message and delivered it to the recipient via a 3rd party mail server. go-mail of course can do much more. Check out the in-depth documentation for all the features.

## Full example code

{{< highlight Go "linenos=table" >}}
package main

import ( "github.com/wneessen/go-mail" "log" )

func main() { m := mail.NewMsg() if err := m.From("toni.sender@example.com"); err != nil { log.Fatalf("failed to set From address: %s", err) } if err := m.To("tina.recipient@example.com"); err != nil { log.Fatalf("failed to set To address: %s", err) } m.Subject("This is my first mail with go-mail!") m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain), mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass")) if err != nil { log.Fatalf("failed to create mail client: %s", err) } if err := c.DialAndSend(m); err != nil { log.Fatalf("failed to send mail: %s", err) } }
{{< /highlight >}}