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

Client `Option` are functions that can be used as optional arguments for the `NewClient()` methods to override the default vaules of the returned `Client`.

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

`WithDebugLog` enables debug logging of SMTP traffic on the `Client`. When enabled, any SMTP communication from the client to the server and vice-versa is logged to `os.Stderr`. 启用后，客户端与服务器之间的任何 SMTP 通信都会记录到 `os.Stderr`。 启用后，客户端与服务器之间的任何 SMTP 通信都会记录到 `os.Stderr`。

In the output `C --> S` is the communication from the client to the server and `C <-- S` represents the communication back from the server to the client.

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

The `WithDSN` option function tells the `Client` to request DSNs (if the server supports it) as described in [RFC 1891](https://rfc-editor.org/rfc/rfc1891.html).

DSNs (Delivery Status Notification) are an extension to the SMTP protocol and need to be supported by the sending server. The RFC for DSNs defines different parameters of which we've implemented the once which we think make most sense for go-mail: DSNs (Delivery Status Notification) are an extension to the SMTP protocol and need to be supported by the sending server. The RFC for DSNs defines different parameters of which we've implemented the once which we think make most sense for go-mail: DSN 的 RFC 定义了不同的参数，我们已经实现了我们认为对 go-mail 最有意义的参数：

* The `RET` extension for the `MAIL FROM` command, to let the user specify if a DSN should contain the full mail (`FULL`) or only headers (`HDRS`) of the sent mail.
* The `NOTIFY` extension that allows the user to request a DSN for the different types of allowed situations: `NEVER`, `SUCCESS`, `FAILURE` and `DELAY`

`ENVID` and `ORCPT` are currently not supported but might follow in a later relaese (please open an [issue](https://github.com/wneessen/go-mail/issues/new/choose) if you see usefulness in this).

By default `WithDSN()` sets the `FULL` *Mail From Return Option* and the `SUCCESS` and `FAILURE` *Recipient Notify Options*. If you like to use other settings for the DSN, please see the documentation for [WithDSNMailReturnType](#withdsnmailreturntype) and [WithDSNRcptNotifyType](#withdsnrcptnotifytype) 如果您想使用其他 DSN 设置，请参阅 [WithDSNMailReturnType](#withdsnmailreturntype) 和 [WithDSNRcptNotifyType](#withdsnrcptnotifytype) 的文档。 如果您想使用其他 DSN 设置，请参阅 [WithDSNMailReturnType](#withdsnmailreturntype) 和 [WithDSNRcptNotifyType](#withdsnrcptnotifytype) 的文档。

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

`WithDSNMailReturnType` enables the `Client` to request DSNs (if the server supports it) as described in the [RFC 1891](https://www.rfc-editor.org/rfc/rfc1891) and set the `MAIL FROM` Return option type to the given `DSNMailReturnOption`

go-mail 已经内置了以下两种 `DSNMailReturnOption` 类型：

* `DSNMailReturnHeadersOnly`: requests that only the headers of the message be returned. \
  See: [RFC 1891, Section 5.3](https://www.rfc-editor.org/rfc/rfc1891#section-5.3) \
  参见：[RFC 1891，第5.3节](https://www.rfc-editor.org/rfc/rfc1891#section-5.3) \
  参见：[RFC 1891，第5.3节](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)
* `DSNMailReturnFull`: requests that the entire message be returned in any "failed" delivery status notification issued for this recipient \
  See: [RFC 1891, Section 5.3](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)

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

`WithDSNRcptNotifyType` enables the `Client` to request DSNs as described in [RFC 1891](https://www.rfc-editor.org/rfc/rfc1891) and sets the `RCPT TO` notify options to the given list of `DSNRcptNotifyOption`

go-mail 已经内置了以下 `DSNRcptNotifyOption` 类型：

* `DSNRcptNotifyNever`: requests that a DSN not be returned to the sender under any conditions. \
  See: [RFC 1891, Section 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1) \
  参见：[RFC 1891，第5.1节](https://www.rfc-editor.org/rfc/rfc1891#section-5.1) \
  参见：[RFC 1891，第5.1节](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifySuccess`：请求在成功传递时发出 DSN\
  参见：[RFC 1891，第5.1节](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifyFailure`：请求在传递失败时发出 DSN\
  参见：[RFC 1891，第5.1节](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifyDelay`: indicates the sender's willingness to receive "delayed" DSNs. Delayed DSNs may be issued if delivery of a message has been delayed for an unusual amount of time (as determined by the MTA at which the message is delayed), but the final delivery status (whether successful or failure) cannot be determined. The absence of the DELAY keyword in a NOTIFY parameter requests that a "delayed" DSN NOT be issued under any conditions. See: [RFC 1891, Section 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1) 如果消息的传递已经被延迟了不寻常的时间（由消息被延迟的 MTA 确定），但无法确定最终的传递状态（无论成功还是失败），则可以发出延迟的 DSN。 如果消息的传递已经被延迟了不寻常的时间（由消息被延迟的 MTA 确定），但无法确定最终的传递状态（无论成功还是失败），则可以发出延迟的 DSN。 在 NOTIFY 参数中缺少 DELAY 关键字会要求在任何情况下都不发出“延迟”的 DSN。 \
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

`WithHELO` 指示 `Client` 使用提供的字符串作为 HELO/EHLO 问候主机。 `WithHELO` instructs the `Client` to use the provided string as HELO/EHLO greeting host. By default the `Client` will use Go's `os.Hostname()` method to get the local hostname and use that for the HELO/EHLO greeting. `WithHELO` will override this. `WithHELO` 将覆盖此设置。 `WithHELO` 将覆盖此设置。