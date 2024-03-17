---
title: 邮件发送客户端
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

要创建新的 `Client`，您可以使用 `NewClient()` 方法。 作为第一个参数，它需要发送 SMTP 服务器的主机名。 您可以选择提供一系列 `Option` 函数。 这些选项函数可用于覆盖 `Client` 的默认设置。

有关所有可用选项的详细信息，请查看 [Options](options) 文档。

## Client

{{< tabs "Client" >}}
{{< tab "Signature" >}}

```go
type Client struct {
// 包含过滤或未导出字段
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

`Close()` closes the connection to the SMTP server the `Client` is connected to. It returns an `error` in case the `Client` has no active connection or if closing the connection fails. 如果 `Client` 没有活动连接或关闭连接失败，则返回 `error`。 如果 `Client` 没有活动连接或关闭连接失败，则返回 `error`。

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

The `DialAndSend()` method is an alias for [DialAndSendWithContext()](#dialandsendwithcontext) with a default `context.Background` context. `DialAndSend()` takes a list of `Msg` pointer as argument(s) and returns an `error` in case any of the performed actions fails. `DialAndSend()` 接受一个或多个 `Msg` 指针作为参数，并在执行任何操作失败时返回 `error`。 `DialAndSend()` 接受一个或多个 `Msg` 指针作为参数，并在执行任何操作失败时返回 `error`。

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

`DialAndSendWithContext()` 是 `Client` 上的一站式快捷方法。 The `DialAndSendWithContext()` is a one-for-all shortcut method on the `Client`. Once the `Client` is created, calling the `DialAndSendWithContext()` method will have it connect to the configured server, send out the given mail `Msg` and finalize by closing the connection again.

The first argument of the method is a `context.Context` followed by a list of one or more `Msg` pointers. `DialAndSendWithContext()` does return an `error` in case any of the performed actions fails. The first argument of the method is a `context.Context` followed by a list of one or more `Msg` pointers. `DialAndSendWithContext()` does return an `error` in case any of the performed actions fails. `DialAndSendWithContext()` 在执行任何操作失败时返回 `error`。