---
title: Client
---

In go-mail the `Client` is responsible for the mail delivery with remote mail servers that communicate via the
SMTP protocol.

## `NewClient()`

{{< tabs "NewClient" >}}
{{< tab "Signature" >}}
{{< highlight Go "linenos=table" >}}
func NewClient(string, ...Option) (*Client, error)
{{< /highlight >}}
{{< /tab >}}
{{< tab "Example" >}}
{{< highlight Go "linenos=table" >}}
package main

import (
    "github.com/wneessen/go-mail"
    "log"
)

func main() {
    c, err := mail.NewClient("mail.example.com")
    if err != nil {
        log.Fatal(err)
    }
}
{{< /highlight >}}
{{< /tab >}}
{{< /tabs >}}

To create a new `Client`, you can use the `NewClient()` method. As first argument it requires the hostname of the
sending SMTP server. Optionally you can provide a list of `Option` funcionts. These option functions can be used 
to override the default settings of the `Client`.

Check the [Options](options) documentation for in-depth details to all available Options.
