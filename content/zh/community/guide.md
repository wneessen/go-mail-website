---
title: 社区指南
---

go-mail社区正在壮大，如果您正在阅读此文，那么您也想加入！

{{< toc >}}

## 资源

### 行为准则

在我们的社区中，我们遵循我们的[行为准则](https://github.com/wneessen/go-mail/blob/main/CODE_OF_CONDUCT.md)，并要求每个想要参与的人都要相应地行事。

### 支持和公告渠道

* [Mastodon](https://s.pebcak.de/@go_mail/)：在Mastodon上关注我们，以获取有关go-mail的最新消息
* [go-mail论坛](https://github.com/wneessen/go-mail/discussions)：接收有关go-mail的公告并开始讨论。
* [Github问题](https://github.com/wneessen/go-mail/issues)：如果您要报告错误或请求功能，请使用GitHub问题。请遵守每个存储库的问题模板中指定的规则。
* [Discord](https://discord.gg/dbfQyC4s)：go-mail开发人员和用户在此处实时会面和聊天的地方。

## 贡献

go-mail是一个开源的、社区驱动的项目。我们欢迎任何人加入我们为项目做出贡献。本文档旨在帮助任何希望熟悉项目和开发流程的人。

* [开发新功能](#developing-new-features)
* [修复错误](#fixing-bugs)
* [测试](#testing)
* [文档](#documentation)
* [翻译](#translation)
* [支持](#support)

<!-- https://crwd.in/go-mail //-->

### 开发新功能

我们始终热衷于为go-mail添加新功能。添加新功能的过程如下：

* 在Github的[问题部分](https://github.com/wneessen/go-mail/issues)中检查带有“TODO”或“help wanted”标签的可用问题
* 如果没有找到打开的“TODO”/“help wanted”问题或您想要的功能未涵盖，请为该特定功能打开一个提案问题，并等待项目维护者的“OK”
* 开发之前，请检查问题是否包括以下信息：
  * 增强的目的
  * 增强范围之外的内容
* 如果问题不包括此信息，请随时向打开问题的人请求信息。有时会创建占位符问题并需要更多详细信息
* 在问题上发表评论，说明您希望开发该功能
* 克隆存储库并创建格式为`feature/<issue_number>_<issue_title>`的分支
* 新功能通常需要文档，因此请确保您还添加或更新了文档作为更改的一部分
* 请确保您的代码具有所需的测试覆盖范围
* 一旦功能准备好进行测试，请创建草案PR。请确保PR说明中列出了测试场景和测试用例，并带有复选框，以便其他人知道仍需测试什么
* 一旦所有测试都完成，请从草案中更新PR的状态并留言

{{< hint type=important >}}
未附带相应问题的任何PR可能会被拒绝。
{{< /hint >}}

### 修复错误

修复错误的过程如下：

* 检查[Github问题](https://github.com/wneessen/go-mail/issues)并选择要修复的错误
* 开发之前，请检查问题是否包括以下信息：
  * 受影响的平台范围
  * 重现步骤。有时会打开不是go-mail问题的错误，并且责任在于报告人证明它是具有最小可重现示例的go-mail问题
* 如果问题不包括此信息，请随时向打开问题的人请求信息
* 在问题上发表评论，说明您希望开发修复程序
* 克隆存储库并创建格式为`bugfix/<issue_number>_<issue_title>`的分支
* 一旦修复程序准备好进行测试，请创建草案PR。请确保PR说明中列出了测试场景和测试用例，并带有复选框，以便其他人知道仍需测试什么
* 一旦所有测试都完成，请从草案中更新PR的状态并留言。

{{< hint type=note >}}
没有任何阻止您打开问题并自己解决它，但请注意，所有错误修复都应该进行讨论，因为方法可能会产生意外的副作用。
{{< /hint >}}
  
{{< hint type=important >}}
未附带相应问题的任何PR可能会被拒绝。
{{< /hint >}}


### 测试

测试对于确保项目质量至关重要。有几种情况下，测试可以真正帮助项目：

* 测试是否可以在本地系统上重现错误
* 测试PR以确保它们正常工作

如果您选择测试某人的错误报告是否可以在本地系统上重现，则可以在问题上添加评论，确认这一点，并附上测试程序的输出。

要测试PR，请选择要测试的PR并检查PR说明中是否列出了测试场景。如果没有，请要求打开PR的人提供该列表。一旦确定了有效的测试场景，请在PR上报告您的发现。

如果您需要更多的明确或帮助进行测试，请在[Github论坛](https://github.com/wneessen/go-mail/discussions)或[Discord](https://discord.gg/dbfQyC4s)上提问。

### 文档

虽然我们要求代码中有适当的GoDoc文档注释，但本网站旨在更深入地记录功能和项目本身的文档。

由于文档很难，网站仍处于不完整状态，因此对此的任何贡献都将不胜感激。没有文档的功能被认为是“未完成”的项目，它与代码一样重要。

该网站基于Hugo使用Geekdocs主题构建。它非常简单，基本上由Markdown文件组成。在[网站的存储库](https://github.com/wneessen/go-mail-website)中有有关如何在本地计算机上安装网站的说明。

### 翻译

go-mail项目的默认文档是英文文档。我们使用“Crowdin”工具将其他语言的文档翻译并同步到网站上。您可以[加入我们的项目](https://translations.go-mail.dev)并提交您的翻译以进行贡献。

目前，唯一支持的第二种语言是德语，但我们也热衷于添加其他语言。请通过go-mail-website存储库中的Github问题请求它们。

### 支持

为项目做出贡献的一个很好的方法是帮助那些遇到困难的人。这通常报告为问题或在`#go-mail` [Discord频道](https://discord.gg/dbfQyC4s)上的消息。即使只是澄清问题，也可以真正帮助。有时，当问题得到讨论并得到解决时，我们会将其制作成指南，以帮助其他面临相同问题的人。
