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

Client `Option` are functions that can be used as optional arguments for the `NewClient()` methods to override the default vaules of the returned `Client`.

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

import ( "github.com/wneessen/go-mail" "log" )

func main() { c, err := mail.NewClient("mail.example.com", mail.WithDSN()) if err != nil { log.Fatal(err) } }
{{< /highlight >}}
{{< /tab >}}
{{< /tabs >}}

The `WithDSN` option function tells the `Client` to request DSNs (if the server supports it) as described in [RFC 1891](https://rfc-editor.org/rfc/rfc1891.html).

DSNs (Delivery Status Notification) are an extension to the SMTP protocol and need to be supported by the sending server. The RFC for DSNs defines different parameters of which we've implemented the once which we think make most sense for go-mail:

* The `RET` extension for the `MAIL FROM` command, to let the user specify if a DSN should contain the full mail (`FULL`) or only headers (`HDRS`) of the sent mail.
* The `NOTIFY` extension that allows the user to request a DSN for the different types of allowed situations: `NEVER`, `SUCCESS`, `FAILURE` and `DELAY`

`ENVID` and `ORCPT` are currently not supported but might follow in a later relaese (please open an [issue](https://github.com/wneessen/go-mail/issues/new/choose) if you see usefulness in this).

By default `WithDSN()` sets the `FULL` *Mail From Return Option* and the `SUCCESS` and `FAILURE` *Recipient Notify Options*. If you like to use other settings for the DSN, please see the documentation for [WithDSNMailReturnType](#withdsnmailreturntype) and [WithDSNRcptNotifyType](#withdsnrcptnotifytype)

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

import ( "github.com/wneessen/go-mail" "log" )

func main() { c, err := mail.NewClient("mail.example.com", mail.WithDSNMailReturnType(mail.DSNMailReturnFull)) if err != nil { log.Fatal(err) } }
{{< /highlight >}}
{{< /tab >}}
{{< /tabs >}}

`WithDSNMailReturnType` enables the `Client` to request DSNs (if the server supports it) as described in the [RFC 1891](https://www.rfc-editor.org/rfc/rfc1891) and set the `MAIL FROM` Return option type to the given `DSNMailReturnOption`

go-mail has the following two `DSNMailReturnOption` type already built-in:

* `DSNMailReturnHeadersOnly`: requests that only the headers of the message be returned. \
  See: [RFC 1891, Section 5.3](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)
* `DSNMailReturnFull`: requests that the entire message be returned in any "failed" delivery status notification issued for this recipient \
  See: [RFC 1891, Section 5.3](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)

### WithDSNRcptNotifyType()

{{< tabs "WithDSNRcptNotifyType" >}}
{{< tab "Signature" >}}
{{< highlight Go "linenos=table" >}}
func WithDSNRcptNotifyType(...DSNRcptNotifyOption) Option
{{< /highlight >}}
{{< /tab >}}
{{< tab "Example" >}}
{{< highlight Go "linenos=table" >}}
package main

import ( "github.com/wneessen/go-mail" "log" )

func main() { c, err := mail.NewClient("mail.example.com", mail.WithDSNRcptNotifyType(mail.DSNRcptNotifyFailure, mail.DSNRcptNotifyDelay, mail.DSNRcptNotifySuccess)) if err != nil { log.Fatal(err) } }
{{< /highlight >}}
{{< /tab >}}
{{< /tabs >}}

`WithDSNRcptNotifyType` enables the `Client` to request DSNs as described in [RFC 1891](https://www.rfc-editor.org/rfc/rfc1891) and sets the `RCPT TO` notify options to the given list of `DSNRcptNotifyOption`

go-mail has the following `DSNRcptNotifyOption` types already built-in:

* `DSNRcptNotifyNever`: requests that a DSN not be returned to the sender under any conditions. \
  See: [RFC 1891, Section 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifySuccess`: requests that a DSN be issued on successful delivery \
  See: [RFC 1891, Section 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifyFailure`: requests that a DSN be issued on delivery failure \
  See: [RFC 1891, Section 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifyDelay`: indicates the sender's willingness to receive "delayed" DSNs. Delayed DSNs may be issued if delivery of a message has been delayed for an unusual amount of time (as determined by the MTA at which the message is delayed), but the final delivery status (whether successful or failure) cannot be determined. The absence of the DELAY keyword in a NOTIFY parameter requests that a "delayed" DSN NOT be issued under any conditions. See: [RFC 1891, Section 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)

### WithHELO()

{{< tabs "WithHELO" >}}
{{< tab "Signature" >}}
{{< highlight Go "linenos=table" >}}
func WithHELO(string) Option
{{< /highlight >}}
{{< /tab >}}
{{< tab "Example" >}}
{{< highlight Go "linenos=table" >}}
package main

import ( "github.com/wneessen/go-mail" "log" )

func main() { c, err := mail.NewClient("mail.example.com", mail.WithHELO("test.example.com")) if err != nil { log.Fatal(err) } }
{{< /highlight >}}
{{< /tab >}}
{{< /tabs >}}

`WithHELO` instructs the `Client` to use the provided string as HELO/EHLO greeting host. By default the `Client` will use Go's `os.Hostname()` method to get the local hostname and use that for the HELO/EHLO greeting. `WithHELO` will override this.