---
title: Willkommen in der go-mail-Dokumentation
geekdocNav: false
#geekdocAlign: center
geekdocAnchor: false
---

[![GoDoc](https://godoc.org/github.com/wneessen/go-mail?status.svg)](https://pkg.go.dev/github.com/wneessen/go-mail) [![codecov](https://codecov.io/gh/wneessen/go-mail/branch/main/graph/badge.svg?token=37KWJV03MR)](https://codecov.io/gh/wneessen/go-mail) [![Go Report Card](https://goreportcard.com/badge/github.com/wneessen/go-mail)](https://goreportcard.com/report/github.com/wneessen/go-mail) [![Crowdin](https://badges.crowdin.net/go-mail/localized.svg)](https://crowdin.com/project/go-mail) [![GitHub release](https://img.shields.io/github/v/release/wneessen/go-mail)](https://github.com/wneessen/go-mail/releases/latest) [![Mentioned in Awesome Go](https://awesome.re/mentioned-badge-flat.svg)](https://github.com/avelino/awesome-go) [![#go-mail on Discord](https://img.shields.io/badge/Discord-%23gomail-blue.svg)](https://discord.gg/zSUeBrsFPB) [![REUSE status](https://api.reuse.software/badge/github.com/wneessen/go-mail)](https://api.reuse.software/info/github.com/wneessen/go-mail)
<a href="https://ko-fi.com/D1D24V9IX"><img src="https://uploads-ssl.webflow.com/5c14e387dab576fe667689cf/5cbed8a4ae2b88347c06c923_BuyMeACoffee_blue.png" height="20" alt="buy ma a coffee"></a>

<p align="center"><img src="/go-mail-2.svg" width="250" alt="go-mail logo"/></p>

go-mail ist eine einfach zu benutzende Go-Bibliothek zum Formatieren und Versenden von E-Mails. Es verwerndet einen idomatischen Go-Stil und folgt "Best Practices" mit vernünftigen Standards. Die Bibliothek hängt nur von der Go-Standardbibliothek ab.

go-mail funktioniert wie ein programmatischer E-Mail-Client und bietet viele Methoden und Funktionalitäten, die Du als Standard in einem E-Mail-Programm betrachten würdest.

<div class="btn-centered btn-huge">
{{< button size="huge" relref="getting-started/introduction/" >}}Erste Schritte mit go-mail{{< /button >}}
</div>

## Funktions-Highlights

{{< columns >}}

### Nur abhängig von der Standard-Bibliothek

go-mail benötigt keine Module von Drittanbietern und basiert ausschließlich auf der Go-Standardbibliothek

<--->

### Modern, idiomatisch

Wir verwenden moderne und idiotmatische Go-Standards mit dieser Bibliothek und folgen den modernsten bewährten -Verfahren mit vernünftigen Standardeinstellungen

<--->

### Vollständiger TLS-Support

go-mail unterstützt implizite STARTTLS mit verschiedenen Richtlinien sowie explizite SSL/TLS für Verbindungen zum Senden von Mail-Servern

{{< /columns >}}

{{< columns >}}

### Kontexte

Wir verwenden Go-Kontexte für einen besseren Kontrollfluss und die Handhabung von Timeouts/Abbrüchen

<--->

### SMTP-Authentifizierung

Unterstützung für drei gängige SMTP-Authentifizierungsmechanismen (LOGIN, PLAIN, CRAM-MD5) sowie für benutzerdefinierte Authentifizierungen.

<--->

### Überprüfung von E-Mail-Adressen

go-mail folgt RFC5322 und validiert die angegebenen Mailadressen

{{< /columns >}}

{{< columns >}}

### Unterstützung allgemeiner Mail-Header

go-mail bringt Generatoren für viele gängige Mail-Header mit (Message-ID, Datum, Bulk-Precedence, Priority, etc.)

<--->

### Wiederverwendung von Verbindungen

Du kannst mehrere Mails über dieselbe SMTP-Verbindung senden

<--->

### Anhänge/Einbettungen

Volle Unterstützung für Anhänge und Inline-Einbettungen aus verschiedenen Quellen (lokales Dateisystem, `io.Reader` oder `embed.FS`)

{{< /columns >}}

{{< columns >}}

### Kodierungen und Inhaltsarten

go-mail unterstützt standardmäßig verschiedene Kodierungen und Inhaltstypen

<--->

### Middlewares

Middleware-Unterstützung für Bibliotheken von Drittanbietern, um E-Mail-Nachrichten an ihre Bedürfnisse anzupassen

<--->

### Sendmail und Dateispeicherung

Support for sending mail messages through a local sendmail installation as well as output to local files (e. g. as `.eml` files to disk to open them in a MUA)

{{< /columns >}}

{{< columns >}}

### MDNs and DSNs

go-mail brings support for requestng MDNs (RFC 8098) and DSNs (RFC 1891)

<--->

### Template support

Support for Go's `html/template` and `text/template` (as message body, alternative part or attachment/emebed)

<--->

{{< /columns >}}

## Support
We have a support and general discussion channel on the Gophers Discord server. Find us at: [#go-mail](https://discord.gg/zSUeBrsFPB)

