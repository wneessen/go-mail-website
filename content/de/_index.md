---
title: Willkommen zur go-mail Dokumentation
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

<p align="center"><img src="/go-mail-2.svg" width="250" alt="go-mail logo"/></p>

go-mail ist ein einfach zu bedienendes Go-Modul zum Formatieren und Versenden von Mails. Es setzt auf einen
idiomatischen Go-Stil und folgt "Best Practices" mit vernünftigen Voreinstellungen. Die Bibliothek hängt 
nur von der Go-Standardbibliothek ab.

go-mail funktioniert wie ein programmatischer E-Mail-Client und bietet viele Methoden und Funktionalitäten, 
die man in einem MUA (Mail User Agent) als Standard betrachten würde.

<div class="btn-centered btn-huge">
{{< button size="huge" relref="usage/getting-started/" >}}Erste Schritte mit go-mail{{< /button >}}
</div>

## Besondere Merkmale

{{< columns >}}

### Keine Abhängigkeiten

go-mail benötigt keine Module von Drittanbietern und basiert rein auf der Go-Standardbibliothek

<--->

### Modernes, idiomatisches Go

Wir setzen in dieser Bibliothek auf moderne und idiotische Go-Standards und folgen den aktuellen
"Best Practices" mit sicheren Voreinstellungen

<--->

### Vollständige TLS-Unterstützung

go-mail unterstützt sowohl implizites STARTTLS mit verschiedenen Richtlinien als auch explizites SSL/TLS 
für Verbindungen zu sendenden Mailservern

{{< /columns >}}

{{< columns >}}

### Kontext-Unterstützung

Wir verwenden Go-Kontexte für einen besseren Kontrollfluss und die Handhabung von Zeitüberschreitungen/Abbrüchen

<--->

### SMTP-Authentifizierung

Unterstützung für drei gängige SMTP-Authentifizierungsmechanismen (LOGIN, PLAIN, CRAM-MD5) sowie für 
benutzerdefinierte Authentifizierungen.

<--->

### Überprüfung von E-Mail-Adressen

go-mail folgt RFC5322 und validiert die übergebenen Mailadressen

{{< /columns >}}

{{< columns >}}

### Unterstützung allgemeiner Mail-Header

go-mail bringt Generatoren für viele gängige Mail-Header mit (Message-ID, Date, Bulk-Precedence, Priority, etc.)

<--->

### Wiederverwendung von Verbindungen

Du kannst mehrere Mails über dieselbe SMTP-Verbindung versenden

<--->

### Anhänge/Einbettungen

Volle Unterstützung für Anhänge und Inline-Einbettungen aus verschiedenen Quellen (lokales Dateisystem,
`io.Reader` oder `embed.FS`)

{{< /columns >}}

{{< columns >}}

### Kodierungen und Inhaltstypen

go-mail unterstützt von Haus aus verschiedene Kodierungs- und Inhaltstypen

<--->

### Middleware

Middleware-Unterstützung für Bibliotheken von Drittanbietern zur Anpassung der E-Mail-Nachrichten an Deine 
Bedürfnisse

<--->

### Sendmail und Dateispeicherung

Unterstützung für den Versand von E-Mail-Nachrichten über eine lokale sendmail-Installation sowie die Ausgabe in 
lokale Dateien (z. B. als `.eml`-Dateien auf der Festplatte, um sie in einem MUA zu öffnen)

{{< /columns >}}

{{< columns >}}

### MDNs und DSNs

go-mail bietet Unterstützung für die Anfrage von MDNs (RFC 8098) und DSNs (RFC 1891)

<--->

### Template-Unterstüzung

Unterstützung für Go's `html/template` und `text/template` (als Nachrichtentext, alternativer Teil oder 
Anhang/Embedded)

<--->

{{< /columns >}}

## Brauchst Du Hilfe?
Wir haben einen Support- und allgemeinen Diskussionskanal auf dem Gophers Discord-Server. Finde uns 
hier: [#go-mail](https://discord.gg/zSUeBrsFPB)

