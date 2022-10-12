---
title: Getting started
weight: -20
---

This short tutorial shows you how to get up and running with go-mail from installation to sending your first
mail.

<!--more-->

{{< toc >}}

## Requirements

go-mail requires a working Go installation (Version 1.16+). Download Go from the
[Go Downloads Page](https://go.dev/dl/).

## Installation

go-mail can be installed using the Go module installation mechanism via the `go install` command.

To install the latest version of go-mail simply issue the following command:

```Shell
$ go install github.com/wneessen/go-mail@latest
```

## Your first program

go-mail consists of two main components. The `Msg` which represents the mail message and the `Client` which takes
care of the mail delivery via a SMTP service.

### Create a new message

First let's create a new `Msg` using the `NewMsg()` method and assign a sender address as well as a recipient
address.

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

In this little code snippet, first and foremost we import go-mail into our project. See the `import` statement 
in line 4. Next we create a new message in line 9. Lines 10 and 13 set the sender and recipient addresses. 
Since go-mail makes sure that you are providing valid mail addresses, we return an `error`. This way we can 
make sure that the provided address is accepted by go-mail and will not cause problems later on.

Next we want to set a subject line for our message and fill the mail body with some content.
{{< highlight Go "linenos=table" >}}
m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!")
{{< /highlight >}}

The first argument for `SetBodyString()` is a content type we need to provide. In our example the 
`mail.TypeTextPlain` basically represents a `text/plain` content time - meaning a plain text mail body.