---
title: Community Guide
---

The go-mail community is growing and if you're reading this, chances are that you want to join, too!

{{< toc >}}

## Resources

### Code of Conduct

In our community, we follow our [Code of Conduct](https://github.com/wneessen/go-mail/blob/main/CODE_OF_CONDUCT.md) and
ask everybody who likes to participate to act accordingly.

### Support- and annoucement channels

* [Twitter](https://twitter.com/gomail_dev): Follow us on Twitter to receive recent news about go-mail
* [go-mail forum](https://github.com/wneessen/go-mail/discussions): Receive announcements and 
  start discussions about go-mail.
* [Github issues](https://github.com/wneessen/go-mail/issues): If you have a bug to report or feature to request, please 
  use GitHub issues. Please respect the rules specified in each repository's issue template.
* [Discord](https://discord.gg/zSUeBrsFPB): A place for go-mail devs and users to meet and chat in real time.

## Contributing

go-mail is an open source, community driven project. We welcome anyone to join us in contributing to the project. This
documentation is aimed at anyone wishing to get familiar with the project and the development processes.

* [Developing new features](#developing-new-features)
* [Fixing bugs](#fixing-bugs)
* [Testing](#testing)
* [Documenation](#documentation)
* [Translation](#translation)
* [Support](#support)

<!-- https://crwd.in/go-mail //-->

### Developing New Features

We are always keen to add features to go-mail. The process for adding new features are as follows:

* Check the [issue section on Github](https://github.com/wneessen/go-mail/issues) for available issues with the "TODO"
  or "help wanted" tag
* If no open "TODO"/"help wanted" issue is found or the feature you have in mind is not covered, please open a proposal
  issue for that specific feature and wait for the "OK" from the project maintainers
* Before developing, check that the issue includes the following information:
  * The purpose of the enhancement 
  * What is out of scope for the enhancement 
* If the issue does not include this information, feel free to request the information from the person who opened the
  issue. Sometimes placeholder issues are created and require more details
* Comment on the issue stating if you wish to develop the feature
* Clone the repository and create a branch with the format `feature/<issue_number>_<issue_title>`
* New features often require documentation so please ensure you have also added or updated the documentation as part of
  the changes
* Please make sure that your code has the required test coverage
* Once the feature is ready for testing, create a draft PR. Please ensure the PR description has the test scenarios and
  test cases listed with checkmarks, so that others can know what still needs to be tested
* Once all the testing is completed, please update the status of the PR from draft and leave a message

{{< hint type=important >}}
Any PRs opened without a corresponding issue may be rejected.
{{< /hint >}}

### Fixing bugs

The process for fixing bugs are as follows:

* Check the [Github issues](https://github.com/wneessen/go-mail/issues) and select a bug to fix
* Before developing, check that the issue includes the following information:
  * The scope of the issue including platforms affected
  * The steps to reproduce. Sometimes bugs are opened that are not go-mail issues and the onus is on the reporter to prove
    that it is a go-mail issue with a minimal reproducible example
* If the issue does not include this information, feel free to request the information from the person who opened the
  issue
* Comment on the issue stating you wish to develop a fix
* Clone the repository and create a branch with the format `bugfix/<issue_number>_<issue_title>`
* Once the fix is ready for testing, create a draft PR. Please ensure the PR description has the test scenarios and test
  cases listed with checkmarks, so that others can know what still needs to be tested 
* Once all the testing is completed, please update the status of the PR from draft and leave a message.

{{< hint type=note >}}
There is nothing stopping you from opening a issue and working on it yourself, but please be aware that all bugfixes
should be discussed as the approach may have unintended side effects.
{{< /hint >}}
  
{{< hint type=important >}}
Any PRs opened without a corresponding issue may be rejected.
{{< /hint >}}


### Testing

Testing is vitally important to ensure quality in the project. There are a couple of scenarios where testing can really
help the project:

* Testing if a bug is reproducible on your local system
* Testing PRs to ensure that they work correctly

If you chose to test if someone's bug report is reproducible on your local system, then feel free to add a comment on
the issue confirming this with the output of your test program.

To test PRs, choose a PR to test and check if the PR description has the testing scenarios listed. If not, please ask
the person who opened the PR to provide that list. Once you have determined a valid test scenario, please report your
findings on the PR.

If you ever need more clarity or help on testing, please ask a question in
the [Github forum](https://github.com/wneessen/go-mail/discussions) or on [Discord](https://discord.gg/zSUeBrsFPB).

### Documentation

While we require proper GoDoc documenation comments in the code, this website is meant as more in-depth documenation
of features and the project itself.

Since documenattion is hard and the website is still in an incomplete state, any contribution to this is greatly
appreciated. Features without documentation are condidered "unfinished" to the project, it's as important as the code.

The website is built on Hugo using the Geekdocs theme. It's very simple and basically consists of markdown files. There
are instructions on how to install the website on your local computer in
the [website's repository](https://github.com/wneessen/go-mail-website).

### Translation

The default documents of the go-mail project are English documents. We use the "Crowdin" tool to translate documents in
other languages and synchronize them to the website. You can [join our project](https://crwd.in/go-mail) and submit 
your translations to make contributions.

Currently the only supported 2nd language is German, but we are keen to add other languages as well. Please request
them via a Github issue in the go-mail-website repository.

### Support

A great way to contribute to the project is to help others who are experiencing difficulty. This is normally reported as
a issue or a message on the `#go-mail` [Discord channel](https://discord.gg/zSUeBrsFPB). Even just clarifying the issue
can really help out. Sometimes, when an issue is discussed and gets resolved, we create a guide out of it to help others
who face the same issues.

