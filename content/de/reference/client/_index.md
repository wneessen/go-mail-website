---
title: The mail delivery client
---

In go-mail the `Client` is responsible for the mail delivery with remote mail servers that communicate via the SMTP protocol.

{{< toc >}}

## NewClient()

{{< tabs "NewClient" >}}
{{< tab "Signature" >}}
```go
func NewClient(string, ...Option) (*Client, error)
```
{{< /tab >}}
{{< tab "Example" >}}
```go
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
```
{{< /tab >}}
{{< /tabs >}}

To create a new `Client`, you can use the `NewClient()` method. As first argument it requires the hostname of the sending SMTP server. Optionally you can provide a list of `Option` funcionts. These option functions can be used to override the default settings of the `Client`.

Check the [Options](options) documentation for in-depth details to all available Options.

## Client

{{< tabs "Client" >}}
{{< tab "Signature" >}}
```go
type Client struct {
    // contains filtered or unexported fields
}
```
{{< /tab >}}
{{< /tabs >}}

### Close()

### DialAndSend()

### DialAndSendWithContext()