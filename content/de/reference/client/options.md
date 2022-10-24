---
title: Client-Optionen
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

Client `Option` sind Funktionen, die als optionale Argumente für die `NewClient()` Methoden verwendet werden können, um die Standardeinstellungen des zurückgegebenen `Client` zu überschreiben.

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

Die `WithDSN` Optionsfunktion weist den `Client` an, DSNs anzufordern (wenn der Server sie unterstützt), wie in [RFC 1891](https://rfc-editor.org/rfc/rfc1891.html) beschrieben.

DSNs (Delivery Status Notification) sind eine Erweiterung des SMTP-Protokolls und müssen von dem sendenden Server unterstützt werden. Der RFC für DSNs definiert verschiedene Parameter, von denen wir einmal die implementiert haben, die wir für go-mail für am sinnvollsten halten:

* Die `RET`-Erweiterung für den `MAIL FROM`-Befehl, damit der Benutzer angeben kann, ob ein DSN die vollständige Mail (`FULL`) oder nur die Kopfzeilen (`HDRS`) der gesendeten Mail enthalten soll.
* Die `NOTIFY`-Erweiterung, die es dem Benutzer ermöglicht, einen DSN für die verschiedenen Arten von erlaubten Situationen anzufordern: `NEVER`, `SUCCESS`, `FAILURE` und `DELAY`

`ENVID` und `ORCPT` werden derzeit nicht unterstützt, könnten aber in einer späteren Version folgen (bitte eröffne ein [Issue](https://github.com/wneessen/go-mail/issues/new/choose), wenn du darin einen Nutzen siehst).

Standardmäßig setzt `WithDSN()` die `FULL` *Mail From Return Option* und die `SUCCESS` und `FAILURE` *Recipient Notify Optionen*. Wenn du andere Einstellungen für den DSN verwenden möchtest, lies bitte die Dokumentation für [WithDSNMailReturnType](#withdsnmailreturntype) und [WithDSNRcptNotifyType](#withdsnrcptnotifytype)

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

`WithDSNMailReturnType` ermöglicht es dem `Client`, DSNs anzufordern (wenn der Server es unterstützt), wie in der [RFC 1891](https://www.rfc-editor.org/rfc/rfc1891) beschrieben und den `MAIL FROM` Rückgabeoptionstyp auf die angegebene `DSNMailReturnOption`

go-mail hat die folgenden zwei `DSNMailReturnOption` Typen bereits eingebaut:

* `DSNMailReturnHeadersOnly`: verlangt, dass nur die Kopfzeilen der Nachricht zurückgegeben werden. \
  Siehe: [RFC 1891, Abschnitt 5.3](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)
* `DSNMailReturnFull`: fordert an, dass die gesamte Nachricht in jeder "fehlgeschlagenen" Zustellungsstatus-Benachrichtigung, die für diesen Empfänger ausgegeben wird, zurückgegeben wird \
  Siehe: [RFC 1891, Abschnitt 5.3](https://www.rfc-editor.org/rfc/rfc1891#section-5.3)

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

`WithDSNRcptNotifyType` ermöglicht dem `Client`, DSNs wie in [RFC 1891](https://www.rfc-editor.org/rfc/rfc1891) beschrieben anzufordern und setzt die `RCPT TO` Notify-Optionen auf die angegebene Liste von `DSNRcptNotifyOption`

go-mail hat die folgenden `DSNRcptNotifyOption` Typen bereits eingebaut:

* `DSNRcptNotifyNever`: fordert, dass ein DSN unter keinen Umständen an den Absender zurückgeschickt wird. \
  Siehe: [RFC 1891, Abschnitt 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifySuccess`: fordert an, dass ein DSN bei erfolgreicher Übergabe ausgegeben wird \
  Siehe: [RFC 1891, Abschnitt 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifyFailure`: fordert an, dass ein DSN bei Zustellungsfehler ausgegeben wird \
  Siehe: [RFC 1891, Abschnitt 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)
* `DSNRcptNotifyDelay`: gibt die Bereitschaft des Senders an, "verzögerte" DSNs zu empfangen. Verspätete DSNs können ausgestellt werden, wenn sich die Zustellung einer Nachricht um eine ungewöhnliche Zeitspanne verzögert hat (wie vom MTA bestimmt, bei dem die Nachricht verzögert wurde), aber der endgültige Zustellungsstatus (ob erfolgreich oder nicht) nicht ermittelt werden kann. Das Fehlen des Schlüsselworts DELAY in einem NOTIFY-Parameter bedeutet, dass unter keinen Umständen ein "verzögerter" DSN ausgegeben werden darf. Siehe: [RFC 1891, Abschnitt 5.1](https://www.rfc-editor.org/rfc/rfc1891#section-5.1)

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

`WithHELO` weist den `Client` an, die angegebene Zeichenkette als HELO/EHLO Begrüßungshost zu verwenden. Standardmäßig verwendet der `Client` die Methode `os.Hostname()` von Go, um den lokalen Hostnamen zu ermitteln und diesen für die HELO/EHLO-Begrüßung zu verwenden. `WithHELO` wird dies außer Kraft setzen.