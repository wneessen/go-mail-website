---
title: 简介
weight: -20
---

This short tutorial shows you how to get up and running with go-mail from installation to sending your first mail.

<!--more-->

{{< toc >}}

## 要求

go-mail requires a working Go installation (Version 1.16+). Download Go from the [Go Downloads Page](https://go.dev/dl/). 从[Go下载页面](https://go.dev/dl/)下载Go。 从[Go下载页面](https://go.dev/dl/)下载Go。

## 安装

可以使用Go模块安装机制通过`go get`命令安装go-mail。

To install the latest version of go-mail, enter your project folder and simply import the module by issuing the following command:

```shell
$ go get github.com/wneessen/go-mail
```

## 发送您的第一封邮件

go-mail由两个主要组件组成。 go-mail consists of two main components. The `Msg` which represents the mail message and the `Client` which takes care of the mail delivery via a SMTP service.

### 创建新消息

First let's create a new `Msg` using the `NewMsg()` method and assign a sender address as well as a recipient address.

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

在这个小代码片段中，首先我们将go-mail导入我们的项目。 请参见[第4行](#hl-1-4)中的`import`语句。 接下来，我们在[第9行](#hl-1-9)中创建了一个新消息。 第[10](#hl-1-10)和[13](#hl-1-13)行设置了发件人和收件人地址。 由于go-mail确保您提供的是有效的邮件地址，因此我们返回一个`error`。 这样我们就可以确保go-mail接受提供的地址，并且不会在以后引起问题。

接下来，我们要为我们的邮件设置一个主题行，并填充邮件正文内容。

```go
m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!")
```

The first argument for `SetBodyString()` is a content type we need to provide. In our example the `mail.TypeTextPlain` basically represents a `text/plain` content type - meaning a plain text mail body. 在我们的示例中，`mail.TypeTextPlain`基本上表示`text/plain`内容类型-表示纯文本邮件正文。 在我们的示例中，`mail.TypeTextPlain`基本上表示`text/plain`内容类型-表示纯文本邮件正文。

### 发送邮件

现在我们已经准备好发送邮件消息了，让我们将其发送出去。 Now that we have our mail message ready to go, let's bring it on the way and send it out. For this we'll use the `Client`, which handles the SMTP transmission.

```go
c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain), 
    mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
if err != nil {
    log.Fatalf("failed to create mail client: %s", err)
}
```

In this example we connect to the mail server with the hostname `smtp.example.com` and provide the `Client` with a couple of options like the port we want to connect to, the fact that we want to use `SMTP PLAIN` for authentication and the username and password.

最后，我们告诉客户端交付邮件。

```go
if err := c.DialAndSend(m); err != nil {
    log.Fatalf("failed to send mail: %s", err)
}
```

The `DialAndSend()` method takes care of establishing the connection and sending out the mail. You have the option to call them separately as well, but we won't need this for the quick example. 您也可以分别调用它们，但是我们不需要快速示例。 您也可以分别调用它们，但是我们不需要快速示例。

## 结论

那很简单，不是吗？ 您成功地准备了一封邮件消息，并通过第三方邮件服务器将其发送给收件人。 当然，go-mail可以做更多。 查看详细文档以获取所有功能。

## 完整示例代码

```go
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
    m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") I certainly do!")
    c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain),
        mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
    if err != nil {
        log.Fatalf("failed to create mail client: %s", err)
    }
    if err := c.DialAndSend(m); err != nil {
        log.Fatalf("failed to send mail: %s", err)
    }
}
    m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") I certainly do!")
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
    m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") I certainly do!")
    c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain),
        mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
    if err != nil {
        log.Fatalf("failed to create mail client: %s", err)
    }
    if err := c.DialAndSend(m); err != nil {
        log.Fatalf("failed to send mail: %s", err)
    }
}
    m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") I certainly do!")
    c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain),
        mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
    if err != nil {
        log.Fatalf("failed to create mail client: %s", err)
    }
    if err := c.DialAndSend(m); err != nil {
        log.Fatalf("failed to send mail: %s", err)
    }
}
    m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") I certainly do!")
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
    m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") I certainly do!")
    c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain),
        mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
    if err != nil {
        log.Fatalf("failed to create mail client: %s", err)
    }
    if err := c.DialAndSend(m); err != nil {
        log.Fatalf("failed to send mail: %s", err)
    }
}
    m.Subject("This is my first mail with go-mail!")
m.SetBodyString(mail.TypeTextPlain, "Do you like this mail? I certainly do!") I certainly do!")
    c, err := mail.NewClient("smtp.example.com", mail.WithPort(25), mail.WithSMTPAuth(mail.SMTPAuthPlain),
        mail.WithUsername("my_username"), mail.WithPassword("extremely_secret_pass"))
    if err != nil {
        log.Fatalf("failed to create mail client: %s", err)
    }
    if err := c.DialAndSend(m); err != nil {
        log.Fatalf("failed to send mail: %s", err)
    }
}
```