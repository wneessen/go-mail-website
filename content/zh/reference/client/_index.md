---
title: 邮件发送客户端
---

在 go-mail 中，`Client` 负责通过 SMTP 协议与远程邮件服务器进行邮件传递。

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

`Close()` 关闭 `Client` 连接到的 SMTP 服务器的连接。 如果 `Client` 没有活动连接或关闭连接失败，则返回 `error`。

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

`DialAndSend()` 方法是 [DialAndSendWithContext()](#dialandsendwithcontext) 的别名，使用默认的 `context.Background` 上下文。 `DialAndSend()` 接受一个或多个 `Msg` 指针作为参数，并在执行任何操作失败时返回 `error`。

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

`DialAndSendWithContext()` 是 `Client` 上的一站式快捷方法。 一旦创建了 `Client`，调用 `DialAndSendWithContext()` 方法将使其连接到配置的服务器，发送给定的邮件 `Msg`，然后通过再次关闭连接来完成。

该方法的第一个参数是 `context.Context`，后跟一个或多个 `Msg` 指针。 `DialAndSendWithContext()` 在执行任何操作失败时返回 `error`。