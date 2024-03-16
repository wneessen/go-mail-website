---
title: 客户端选项
---

{{< toc >}}

## Option

{{< tabs "Options" >}}
{{< tab "Signature" >}}
```go
type Option func(*Client) error
```
{{< /tab >}}
{{< /tabs >}}

`Option` 是可选参数函数，可以用作 `NewClient()` 方法的可选参数，以覆盖返回的 `Client` 的默认值。

### WithDebugLog()
{{< tabs "WithDebugLog" >}}
{{< tab "Signature" >}}
```go
func WithDebugLog() Option
```
{{< /tab >}}
{{< tab "Example" >}}
```go
package main

import "github.com/wneessen/go-mail"

func main() {
    c, err := mail.NewClient("mail.example.com", mail.WithDebugLog())
    if err != nil {
        panic(err)
    }
}
```
{{< /tab >}}
{{< tab "Version" >}}
Introduced in [go-mail v0.3.9](https://github.com/wneessen/go-mail/releases/tag/v0.3.9)
{{< /tab >}}
{{< /tabs >}}

`WithDebugLog` 启用 SMTP 流量的调试日志。 启用后，客户端与服务器之间的任何 SMTP 通信都会记录到 `os.Stderr`。

在输出中，`C --> S` 是客户端到服务器的通信，`C <-- S` 表示服务器到客户端的通信。

以下是输出示例：
```
2023/01/15 20:21:18 [DEBUG] C --> S: EHLO client.example.com
2023/01/15 20:21:18 [DEBUG] C <-- S: 250 server.example.com
PIPELINING
SIZE 152428800
ETRN
STARTTLS
AUTH LOGIN PLAIN
AUTH=LOGIN PLAIN
ENHANCEDSTATUSCODES
8BITMIME
DSN
2023/01/15 20:21:18 [DEBUG] C --> S: STARTTLS
2023/01/15 20:21:18 [DEBUG] C <-- S: 220 2.0.0 Ready to start TLS
2023/01/15 20:21:18 [DEBUG] C --> S: EHLO client.example.com
2023/01/15 20:21:18 [DEBUG] C <-- S: 250 server.example.com
PIPELINING
SIZE 152428800
ETRN
AUTH LOGIN PLAIN
AUTH=LOGIN PLAIN
ENHANCEDSTATUSCODES
8BITMIME
DSN
2023/01/15 20:21:18 [DEBUG] C --> S: AUTH LOGIN
2023/01/15 20:21:18 [DEBUG] C <-- S: 334 VXNlcm5hbWU6
2023/01/15 20:21:18 [DEBUG] C --> S: 
2023/01/15 20:21:20 [DEBUG] C <-- S: 535 5.7.8 Error: authentication failed: VXNlcm5hbWU6
```

### WithDSN()

{{< tabs "WithDSN" >}}
{{< tab "Signature" >}}
```go
func WithDSN() Option
```
{{< /tab >}}
{{< tab "Example" >}}
```go
package main

import "github.com/wneessen/go-mail"

func main() {
    c, err := mail.NewClient("mail.example.com", mail.WithDSN())
    if err != nil {
        panic(err)
    }
}
```
{{< /tab >}}
{{< /tabs >}}

`WithDSN` 选项函数告诉 `Client` 请求 DSN（如果服务器支持）如[RFC 1891](https://rfc-editor.org/rfc/rfc1891.html)所述。

DSN（传递状态通知）是 SMTP 协议的扩展，需要发送服务器支持。 DSN 的 RFC 定义了不同的参数，我们已经实现了我们认为对 go-mail 最有意义的参数：

* `MAIL FROM` 命令的 `RET` 扩展，让用户指定 DSN 是否应包含发送邮件的全部内容（`FULL`）或仅包含邮件头（`HDRS`）。
* `NOTIFY` 扩展允许用户为不同类型的允许情况请求 DSN：`NEVER`、`SUCCESS`、`FAILURE` 和 `DELAY`

`ENVID` 和 `ORCPT` 目前不受支持，但可能会在以后的版本中跟进（如果您认为这很有用，请打开一个 [issue](https://github.com/wneessen/go-mail/issues/new/choose)）。

默认情况下，`WithDSN()` 设置 `FULL` 的 *Mail From Return Option* 和 `SUCCESS` 和 `FAILURE` 的 *Recipient Notify Options*。 如果您想使用其他 DSN 设置，请参阅 [WithDSNMailReturnType](#withdsnmailreturntype) 和 [WithDSNRcptNotifyType](#withdsnrcptnotifytype) 的文档。

### WithDSNMailReturnType()
{{< tabs "WithDSNMailReturnType" >}}
{{< tab "Signature" >}}
```go
func WithDSNMailReturnType(DSNMailReturnOption) Option
```
{{< /tab >}}
{{< tab "Example" >}}
```go
package main

import "github.com/wneessen/go-mail"

func main() {
    c, err := mail.NewClient("mail.example.com", mail.WithDSNMailReturnType(mail.DSNMailReturnFull))
    if err != nil {
        panic(err)
    }
}
```
{{< /tab >}}
{{< /tabs >}}

`WithDSNMailReturnType` 启用 `Client` 请求 DSN，如[RFC 1891](https://www.rfc-editor.org/rfc/rfc1891)所述，并将 `MAIL FROM` 返回选项类型设置为给定的 `DSNMailReturnOption`。

go-mail 已经内置了以下两种 `DSNMailReturnOption` 类型：

* `DSNMailReturnHeadersOnly`：请求仅返回邮件的头部。 \
  参见：[RFC 1891，第5.3节](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)
* `DSNMailReturnFull`：请求在为此收件人发出“失败”的传递状态通知时返回整个消息。 \
  参见：[RFC 1891，第5.3节](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)

### WithDSNRcptNotifyType()

{{< tabs "WithDSNRcptNotifyType" >}}
{{< tab "Signature" >}}
```go
func WithDSNRcptNotifyType(...DSNRcptNotifyOption) Option
```
{{< /tab >}}
{{< tab "Example" >}}
```go
package main

import "github.com/wneessen/go-mail"

func main() {
    c, err := mail.NewClient("mail.example.com",
        mail.WithDSNRcptNotifyType(mail.DSNRcptNotifyFailure, mail.DSNRcptNotifyDelay,
            mail.DSNRcptNotifySuccess))
    if err != nil {
        panic(err)
    }
}
```
{{< /tab >}}
{{< /tabs >}}

`WithDSNRcptNotifyType` 启用 `Client` 请求 DSN，如[RFC 1891](https://rfc-editor.org/rfc/rfc1891.html)所述，并将 `RCPT TO` 通知选项设置为给定的 `DSNRcptNotifyOption` 列表。

go-mail 已经内置了以下 `DSNRcptNotifyOption` 类型：

* `DSNRcptNotifyNever`：不要在任何情况下向发送方返回 DSN。 \
  参见：[RFC 1891，第5.1节](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifySuccess`：请求在成功传递时发出 DSN\
  参见：[RFC 1891，第5.1节](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifyFailure`：请求在传递失败时发出 DSN\
  参见：[RFC 1891，第5.1节](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifyDelay`：表示发送方愿意接收“延迟”的 DSN。 如果消息的传递已经被延迟了不寻常的时间（由消息被延迟的 MTA 确定），但无法确定最终的传递状态（无论成功还是失败），则可以发出延迟的 DSN。 在 NOTIFY 参数中缺少 DELAY 关键字会要求在任何情况下都不发出“延迟”的 DSN。 \
  参见：[RFC 1891，第5.1节](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)

### WithHELO()

{{< tabs "WithHELO" >}}
{{< tab "Signature" >}}
```go
func WithHELO(string) Option
```
{{< /tab >}}
{{< tab "Example" >}}
```go
package main

import "github.com/wneessen/go-mail"

func main() {
    c, err := mail.NewClient("mail.example.com", mail.WithHELO("test.example.com"))
    if err != nil {
        panic(err)
    }
}
```
{{< /tab >}}
{{< /tabs >}}

`WithHELO` 指示 `Client` 使用提供的字符串作为 HELO/EHLO 问候主机。 默认情况下，`Client` 将使用 Go 的 `os.Hostname()` 方法获取本地主机名，并将其用于 HELO/EHLO 问候。 `WithHELO` 将覆盖此设置。