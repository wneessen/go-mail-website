---
title: Welcome to the go-mail documentation
geekdocNav: false
#geekdocAlign: center
geekdocAnchor: false
---

[![GoDoc](https://godoc.org/github.com/wneessen/go-mail?status.svg)](https://pkg.go.dev/github.com/wneessen/go-mail)
[![codecov](https://codecov.io/gh/wneessen/go-mail/branch/main/graph/badge.svg?token=37KWJV03MR)](https://codecov.io/gh/wneessen/go-mail)
[![Go Report Card](https://goreportcard.com/badge/github.com/wneessen/go-mail)](https://goreportcard.com/report/github.com/wneessen/go-mail)
[![Build Status](https://api.cirrus-ci.com/github/wneessen/go-mail.svg)](https://cirrus-ci.com/github/wneessen/go-mail)
[![GitHub release](https://img.shields.io/github/v/release/wneessen/go-mail)](https://github.com/wneessen/go-mail/releases/latest)
[![Mentioned in Awesome Go](https://awesome.re/mentioned-badge-flat.svg)](https://github.com/avelino/awesome-go) [![#go-mail on Discord](https://img.shields.io/badge/Discord-%23gomail-blue.svg)](https://discord.gg/zSUeBrsFPB)
[![REUSE status](https://api.reuse.software/badge/github.com/wneessen/go-mail)](https://api.reuse.software/info/github.com/wneessen/go-mail)
<a href="https://ko-fi.com/D1D24V9IX"><img src="https://uploads-ssl.webflow.com/5c14e387dab576fe667689cf/5cbed8a4ae2b88347c06c923_BuyMeACoffee_blue.png" height="20" alt="buy ma a coffee"></a>

<p align="center"><img src="./go-mail-2.svg" width="250" alt="go-mail logo"/></p>

go-mail is an easy to use Go module for formating and sending mails. It uses idiomatic Go style and follows 
best practice with sane defaults. The library only dependends on the Go Standard Library.

go-mail works like a programatic email client and provides lots of methods and functionalities you would consider
standard in a MUA.

<div class="btn-centered">
{{< button size="large" relref="usage/getting-started/" >}}Getting started sending mails with Go{{< /button >}}
</div>

## Middleware
The goal of go-mail is to keep it free from 3rd party dependencies and only focus on things a mail library should
fulfill. Yet, since version v0.2.8 we've added support for middleware on the `Msg` object, allowing 3rd parties to
alter a given mail message to their needs without relying on `go-mail` to support their specific need.

To get our users started with message middleware, we've created a collection of useful middlewares. It can be
found in a seperate repository: [go-mail-middlware](https://github.com/wneessen/go-mail-middleware).

## Feature highlights

{{< columns >}}

### Standard Library dependant

go-mail does not require any third-party modules and only runs on the Go standard library

<--->

### Modern, idiomatic Go

We are using modern and idiotmatic Go standards with this library and follow state-of-the-art best 
practices with sane defaults

<--->

### Easy customization

The look and feel can be easily customized by CSS custom properties (variables), features can be adjusted by Hugo parameters.

{{< /columns >}}

{{< columns >}}

### Full TLS support

go-mail supports implicit STARTTLS with different policies as well as explicit SSL/TLS for connections
to sending mail servers

<--->

### Contexts

We make use of Go contexts for better control flow and timeout/cancelation handling

<--->

### SMTP Authentication

Support for three common SMTP authentication mechanisms (LOGIN, PLAIN, CRAM-MD5) as well as custom
authentications.

{{< /columns >}}

{{< columns >}}

### Mail address validation

go-mail follows RFC5322 and validates the provided mail addresses

<--->

### Common mail header support

go-mail brings generators for lots of common mail headers (Message-ID, Date, Bulk-Precedence, Priority, etc.)

<--->

### Connection reusing

You can send mulitple mails over the same SMTP connection

{{< /columns >}}

{{< columns >}}

### Attachments/Embeds

Full support for attachments and inline embeds from different sources (local file system, `io.Reader` or `embed.FS`)

<--->

### Encodings and content types

go-mail supports different encondings and content types out of the box

<--->

### Middlewares

Middleware supports for 3rd-party libraries to alter mail message to their need

{{< /columns >}}

{{< columns >}}

### Sendmail and file storage

Support for sending mail messages through a local sendmail installation as well as output to 
loacl files (e. g. as `.eml` files to disk to open them in a MUA)

<--->

### MDNs and DSNs

go-mail brings support for requestng MDNs (RFC 8098) and DSNs (RFC 1891)

<--->

### Template support

Support for Go's `html/template` and `text/template` (as message body, alternative part or attachment/emebed)

{{< /columns >}}

## Support
We have a support and general discussion channel on the Gophers Discord server. Find us at: [#go-mail](https://discord.gg/zSUeBrsFPB)

