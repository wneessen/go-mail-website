---
title: 简介
weight: -20
---

这篇简短的教程向您展示了如何从安装到发送第一封邮件使用go-mail。

<!--more-->

{{< toc >}}

## 要求

go-mail需要一个工作的Go安装（版本1.16+）。从[Go下载页面](https://go.dev/dl/)下载Go。

## 安装

可以使用Go模块安装机制通过`go get`命令安装go-mail。

要安装go-mail的最新版本，请进入您的项目文件夹，然后通过发出以下命令导入模块即可：

```shell
$ go get github.com/wneessen/go-mail
```

## 发送您的第一封邮件

go-mail由两个主要组件组成。 `Msg`表示邮件消息，`Client`通过SMTP服务处理邮件传递。

### 创建新消息

首先，让我们使用`NewMsg()`方法创建一个新的`Msg`，并分配一个发件人地址和一个收件人地址。

```go
package main

import (
	"github.com/wneessen/go-mail"
	"log"
)

func main() {
	m := mail.NewMsg()
	if err := m.From("toni.sender@example.com"); err != nil {
		log.Fatalf("failed to set From address: %s", err)
	}
	if err := m.To("tina.recipient@example.com"); err != nil {
		log.Fatalf("failed to set To address: %s", err)
	}
}
```

在这个小代码片段中，首先我们将go-mail导入我们的项目。请参见[第4行](#hl-1-4)中的`import`语句。接下来，我们在[第9行](#hl-1-9)中创建了一个新消息。第[10](#hl-1-10)和[13](#hl-1-13)行设置了发件人和收件人地址。由于go-mail确保您提供的是有效的邮件地址，因此我们返回一个`error`。这样我们就可以确保go-mail接受提供的地址，并且不会在以后引起问题。

接下来，我们要为我们的邮件设置一个主题行，并填充邮件正文内容。

```go
m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!")
```

`SetBodyString()`的第一个参数是我们需要提供的内容类型。在我们的示例中，`mail.TypeTextPlain`基本上表示`text/plain`内容类型-表示纯文本邮件正文。

### 发送邮件

现在我们已经准备好发送邮件消息了，让我们将其发送出去。为此，我们将使用`Client`，它处理SMTP传输。

```go
c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain), 
	mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
if err != nil {
	log.Fatalf("failed to create mail client: %s", err)
}
```

在此示例中，我们使用主机名`smtp.example.com`连接到邮件服务器，并为`Client`提供了一些选项，例如我们要连接的端口，我们要使用`SMTP PLAIN`进行身份验证以及用户名和密码。

最后，我们告诉客户端交付邮件。

```go
if err := c.DialAndSend(m); err != nil {
	log.Fatalf("failed to send mail: %s", err)
}
```

`DialAndSend()`方法负责建立连接并发送邮件。您也可以分别调用它们，但是我们不需要快速示例。

## 结论

那很简单，不是吗？您成功地准备了一封邮件消息，并通过第三方邮件服务器将其发送给收件人。当然，go-mail可以做更多。查看详细文档以获取所有功能。

## 完整示例代码

```go
package main

import (
	"github.com/wneessen/go-mail"
	"log"
)

func main() {
	m := mail.NewMsg()
	if err := m.From("toni.sender@example.com"); err != nil {
		log.Fatalf("failed to set From address: %s", err)
	}
	if err := m.To("tina.recipient@example.com"); err != nil {
		log.Fatalf("failed to set To address: %s", err)
	}
	m.Subject("This is my first mail with go-mail!")
	m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!")
	c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain),
		mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
	if err != nil {
		log.Fatalf("failed to create mail client: %s", err)
	}
	if err := c.DialAndSend(m); err != nil {
		log.Fatalf("failed to send mail: %s", err)
	}
}
