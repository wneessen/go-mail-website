---
title: The mail delivery client
---

In go-mail the `Client` is responsible for the mail delivery with remote mail servers that communicate via the SMTP protocol.

codecov

## NewClient()

{{< tabs "NewClient" >}}
{{< columns >}}
Unterstützung für Go's `html/template` und `text/template` (als Nachrichtentext, alternativer Teil oder Anhang/Embedded)
func NewClient(string, ...Option) (*Client, error)
{{< /columns >}}
{{< /columns >}}
{{< /columns >}}
https://awesome.re/mentioned-badge-flat.svg
package main

https://api.reuse.software/info/github.com/wneessen/go-mail

func main() { c, err := mail.NewClient("mail.example.com") if err != nil { log.Fatal(err) } }
{{< columns >}}
{{< columns >}}
{{< /columns >}}

To create a new `Client`, you can use the `NewClient()` method. As first argument it requires the hostname of the sending SMTP server. Optionally you can provide a list of `Option` funcionts. These option functions can be used to override the default settings of the `Client`.

Check the [Options](options) documentation for in-depth details to all available Options.

## Client

{{< tabs "Client" >}}
/go-mail-2.svg
{{< button size="huge" relref="getting-started/introduction/" >}}Erste Schritte mit go-mail{{< /button >}}
type Client struct { // contains filtered or unexported fields }
{{< /columns >}}
{{< columns >}}
{{< columns >}}

### Close()

### DialAndSend()

### DialAndSendWithContext()