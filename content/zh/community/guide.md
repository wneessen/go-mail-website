---
title: 社区指南
---

go-mail社区正在壮大，如果您正在阅读此文，那么您也想加入！

{{< toc >}}

## 资源

### 行为准则

In our community, we follow our [Code of Conduct](https://github.com/wneessen/go-mail/blob/main/CODE_OF_CONDUCT.md) and ask everybody who likes to participate to act accordingly.

### 支持和公告渠道

* [Mastodon](https://s.pebcak.de/@go_mail/)：在Mastodon上关注我们，以获取有关go-mail的最新消息
* [go-mail forum](https://github.com/wneessen/go-mail/discussions): Receive announcements and start discussions about go-mail.
* [Github issues](https://github.com/wneessen/go-mail/issues): If you have a bug to report or feature to request, please use GitHub issues. Please respect the rules specified in each repository's issue template. 请遵守每个存储库的问题模板中指定的规则。
* [Discord](https://discord.gg/dbfQyC4s)：go-mail开发人员和用户在此处实时会面和聊天的地方。

## 贡献

go-mail is an open source, community driven project. We welcome anyone to join us in contributing to the project. This documentation is aimed at anyone wishing to get familiar with the project and the development processes. 我们欢迎任何人加入我们为项目做出贡献。 本文档旨在帮助任何希望熟悉项目和开发流程的人。

* [开发新功能](#developing-new-features)
* [修复错误](#fixing-bugs)
* [测试](#testing)
* [文档](#documentation)
* [翻译](#translation)
* [支持](#support)

<!-- https://crwd.in/go-mail //-->

### 开发新功能

We are always keen to add features to go-mail. The process for adding new features are as follows: 添加新功能的过程如下：

* Check the [issue section on Github](https://github.com/wneessen/go-mail/issues) for available issues with the "TODO" or "help wanted" tag
* If no open "TODO"/"help wanted" issue is found or the feature you have in mind is not covered, please open a proposal issue for that specific feature and wait for the "OK" from the project maintainers
* 开发之前，请检查问题是否包括以下信息：
  * 增强的目的
  * 增强范围之外的内容
* If the issue does not include this information, feel free to request the information from the person who opened the issue. Sometimes placeholder issues are created and require more details 有时会创建占位符问题并需要更多详细信息
* 在问题上发表评论，说明您希望开发该功能
* 克隆存储库并创建格式为`feature/<issue_number>_<issue_title>`的分支
* New features often require documentation so please ensure you have also added or updated the documentation as part of the changes
* 请确保您的代码具有所需的测试覆盖范围
* 一旦功能准备好进行测试，请创建草案PR。 请确保PR说明中列出了测试场景和测试用例，并带有复选框，以便其他人知道仍需测试什么
* 一旦所有测试都完成，请从草案中更新PR的状态并留言

{{< hint type=important >}}
未附带相应问题的任何PR可能会被拒绝。
{{< /hint >}}

### 修复错误

修复错误的过程如下：

* 检查[Github问题](https://github.com/wneessen/go-mail/issues)并选择要修复的错误
* 开发之前，请检查问题是否包括以下信息：
  * 受影响的平台范围
  * 重现步骤。 The steps to reproduce. Sometimes bugs are opened that are not go-mail issues and the onus is on the reporter to prove that it is a go-mail issue with a minimal reproducible example
* If the issue does not include this information, feel free to request the information from the person who opened the issue
* 在问题上发表评论，说明您希望开发修复程序
* 克隆存储库并创建格式为`bugfix/<issue_number>_<issue_title>`的分支
* Once the fix is ready for testing, create a draft PR. Please ensure the PR description has the test scenarios and test cases listed with checkmarks, so that others can know what still needs to be tested Once the feature is ready for testing, create a draft PR. Please ensure the PR description has the test scenarios and test cases listed with checkmarks, so that others can know what still needs to be tested
* 一旦所有测试都完成，请从草案中更新PR的状态并留言。

{{< hint type=note >}}
There is nothing stopping you from opening a issue and working on it yourself, but please be aware that all bugfixes should be discussed as the approach may have unintended side effects.
{{< /hint >}}

{{< hint type=important >}}
未附带相应问题的任何PR可能会被拒绝。
{{< /hint >}}


### 测试

测试对于确保项目质量至关重要。 Testing is vitally important to ensure quality in the project. There are a couple of scenarios where testing can really help the project:

* 测试是否可以在本地系统上重现错误
* 测试PR以确保它们正常工作

If you chose to test if someone's bug report is reproducible on your local system, then feel free to add a comment on the issue confirming this with the output of your test program.

To test PRs, choose a PR to test and check if the PR description has the testing scenarios listed. If not, please ask the person who opened the PR to provide that list. Once you have determined a valid test scenario, please report your findings on the PR. 如果没有，请要求打开PR的人提供该列表。 一旦确定了有效的测试场景，请在PR上报告您的发现。

If you ever need more clarity or help on testing, please ask a question in the [Github forum](https://github.com/wneessen/go-mail/discussions) or on [Discord](https://discord.gg/dbfQyC4s).

### 文档

While we require proper GoDoc documenation comments in the code, this website is meant as more in-depth documenation of features and the project itself.

Since documenattion is hard and the website is still in an incomplete state, any contribution to this is greatly appreciated. Features without documentation are condidered "unfinished" to the project, it's as important as the code. 没有文档的功能被认为是“未完成”的项目，它与代码一样重要。

该网站基于Hugo使用Geekdocs主题构建。 它非常简单，基本上由Markdown文件组成。 The website is built on Hugo using the Geekdocs theme. It's very simple and basically consists of markdown files. There are instructions on how to install the website on your local computer in the [website's repository](https://github.com/wneessen/go-mail-website).

### 翻译

The default documents of the go-mail project are English documents. We use the "Crowdin" tool to translate documents in other languages and synchronize them to the website. You can [join our project](https://translations.go-mail.dev) and submit your translations to make contributions. 我们使用“Crowdin”工具将其他语言的文档翻译并同步到网站上。 您可以[加入我们的项目](https://translations.go-mail.dev)并提交您的翻译以进行贡献。

Currently the only supported 2nd language is German, but we are keen to add other languages as well. Please request them via a Github issue in the go-mail-website repository. 请通过go-mail-website存储库中的Github问题请求它们。

### 支持

为项目做出贡献的一个很好的方法是帮助那些遇到困难的人。 这通常报告为问题或在`#go-mail` [Discord频道](https://discord.gg/dbfQyC4s)上的消息。 即使只是澄清问题，也可以真正帮助。 有时，当问题得到讨论并得到解决时，我们会将其制作成指南，以帮助其他面临相同问题的人。

