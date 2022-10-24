---
title: Der Client für die Mailzustellung
---

In go-mail ist der `Client` für die Mailzustellung mit entfernten Mailservern zuständig, die über das SMTP-Protokoll kommunizieren.

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

import "github.com/wneessen/go-mail"

func main() {
    c, err := mail.NewClient("mail.example.com")
    if err != nil {
        panic(err)
    }
}
```

{{< /tab >}}
{{< /tabs >}}

Um einen neuen `Client` zu erstellen, kannst du die Methode `NewClient()` verwenden. Als erstes Argument benötigt er den Hostnamen des sendenden SMTP-Servers. Optional kannst du eine Liste von `Option`-Funktionen angeben. Diese Optionsfunktionen können verwendet werden, um die Standardeinstellungen des `Client` zu überschreiben.

In der [Optionen](options) Dokumentation findest du ausführliche Informationen zu allen verfügbaren Optionen.

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

{{< tabs "Close" >}}
{{< tab "Signature" >}}

```go
func (*Client) Close() error
```

{{< /tab >}}
{{< tab "Example" >}}

```go
package main

import "github.com/wneessen/go-mail"

func main() {
    c, err := mail.NewClient("mail.example.com")
    if err != nil {
        panic(err)
    }
    defer c.Close()
}
```

{{< /tab >}}
{{< /tabs >}}

`Close()` schließt die Verbindung zu dem SMTP-Server, mit dem der `Client` verbunden ist. Sie gibt einen `error` zurück, wenn der `Client` keine aktive Verbindung hat oder wenn das Schließen der Verbindung fehlschlägt.

### DialAndSend()

{{< tabs "DialAndSend" >}}
{{< tab "Signature" >}}

```go
func (*Client) DialAndSend(...*Msg) error
```

{{< /tab >}}
{{< tab "Example" >}}

```go
package main

import "github.com/wneessen/go-mail"

func main() {
    from := "Toni Tester <toni@example.com>"
    to := "Alice <alice@example.com>"
    server := "mail.example.com"

    m := mail.NewMsg()
    if err := m.From(from); err != nil {
        panic(err)
    }
    if err := m.To(to); err != nil {
        panic(err)
    }
    m.Subject("This is a great subject")

    c, err := mail.NewClient(server)
    if err != nil {
        panic(err)
    }
    if err := c.DialAndSend(m); err != nil {
        panic(err)
    }
}
```

{{< /tab >}}
{{< /tabs >}}

The `DialAndSend()` method is an alias for [DialAndSendWithContext()](#dialandsendwithcontext) with a default `context.Background` context. `DialAndSend()` takes a list of `Msg` pointer as argument(s) and returns an `error` in case any of the performed actions fails.

### DialAndSendWithContext()

{{< tabs "DialAndSendWithContext" >}}
{{< tab "Signature" >}}

```go
func (*Client) DialAndSendWithContext(context.Context, ...*Msg) error
```

{{< /tab >}}
{{< tab "Example" >}}

```go
package main

import "github.com/wneessen/go-mail"

func main() {
    from := "Toni Tester <toni@example.com>"
    to := "Alice <alice@example.com>"
    server := "mail.example.com"

    m := mail.NewMsg()
    if err := m.From(from); err != nil {
        panic(err)
    }
    if err := m.To(to); err != nil {
        panic(err)
    }
    m.Subject("This is a great subject")

    c, err := mail.NewClient(server)
    if err != nil {
        panic(err)
    }
    ctx := context.Background()
    if err := c.DialAndSendWithContext(ctx, m); err != nil {
        panic(err)
    }
}
```

{{< /tab >}}
{{< /tabs >}}

The `DialAndSendWithContext()` is a one-for-all shortcut method on the `Client`. Once the `Client` is created, calling the `DialAndSendWithContext()` method will have it connect to the configured server, send out the given mail `Msg` and finalize by closing the connection again.

The first argument of the method is a `context.Context` followed by a list of one or more `Msg` pointers. `DialAndSendWithContext()` does return an `error` in case any of the performed actions fails.