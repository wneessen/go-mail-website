---
title: Client options
---

{{< toc >}}

## Option

{{< tabs "Options" >}}
{{< tab "Signature" >}}
{{< highlight Go "linenos=table" >}}
type Option func(*Client) error
{{< /highlight >}}
{{< /tab >}}
{{< /tabs >}}

Client `Option` are functions that can be used as optional arguments for the `NewClient()` methods to override the
default vaules of the returned `Client`.

### WithDSN()

{{< tabs "WithDSN" >}}
{{< tab "Signature" >}}
{{< highlight Go "linenos=table" >}}
func WithDSN() Option
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
    c, err := mail.NewClient("mail.example.com", mail.WithDSN())
    if err != nil {
        log.Fatal(err)
    }
}
{{< /highlight >}}
{{< /tab >}}
{{< /tabs >}}

The `WithDSN` option function tells the `Client` to request DSNs (if the server supports it) as described in 
[RFC 1891](https://rfc-editor.org/rfc/rfc1891.html).

DSNs (Delivery Status Notification) are an extension to the SMTP protocol and need to be supported by 
the sending server. The RFC for DSNs defines different parameters of which we've implemented the once which 
we think make most sense for go-mail:

* The `RET` extension for the `MAIL FROM` command, to let the user specify if a DSN should contain the 
  full mail (`FULL`) or only headers (`HDRS`) of the sent mail. 
* The `NOTIFY` extension that allows the user to request a DSN for the different types of allowed 
  situations: `NEVER`, `SUCCESS`, `FAILURE` and `DELAY`

`ENVID` and `ORCPT` are currently not supported but might follow in a later relaese 
(please open an [issue](https://github.com/wneessen/go-mail/issues/new/choose) if you see usefulness in this).

By default `WithDSN()` sets the `FULL` *Mail From Return Option* and the `SUCCESS` and `FAILURE` 
*Recipient Notify Options*. If you like to use other settings for the DSN, please see the documentation for 
[WithDSNMailReturnType](#withdsnmailreturntype) and [WithDSNRcptNotifyType](#withdsnrcptnotifytype)

### WithDSNMailReturnType()
{{< tabs "WithDSNMailReturnType" >}}
{{< tab "Signature" >}}
{{< highlight Go "linenos=table" >}}
func WithDSNMailReturnType(DSNMailReturnOption) Option
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
    c, err := mail.NewClient("mail.example.com", mail.WithDSNMailReturnType(mail.DSNMailReturnFull))
    if err != nil {
        log.Fatal(err)
    }
}
{{< /highlight >}}
{{< /tab >}}
{{< /tabs >}}

The `WithDSNMailReturnType` is the option you can use if you like to request DSNs but don't want to use the
default `FULL` settings of the `WithDSN()` option.

`WithDSNMailReturnType` takes a `DSNMailReturnOption` as argument. go-mail has the two types already built-in:
* `DSNMailReturnHeadersOnly` requests that only the headers of the message be returned. \
  See: [RFC 1891, Section 5.3](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)
* `DSNMailReturnFull`: requests that the entire message be returned in any "failed" delivery status notification 
  issued for this recipient \
  See: [RFC 1891, Section 5.3](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)

### WithDSNRcptNotifyType()

TBD