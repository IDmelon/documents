---
title: "Overview"
description: ""
lead: ""
date: 2024-07-20T14:40:56+01:00
lastmod: 2024-07-20T14:40:56+01:00
draft: false
images: [ ]
menu:
  docs:
    parent: "weblogin"
type: docs
weight: 321100
toc: true
---

IDmelon WebLogin empowers you to replace your passwords with a single tap of your card on a reader. In addition, this
extension acts as a FIDO platform and provides capability of creating and using passkeys. Note that you need to first
have the latest version of IDmelon Card Reader Driver (v3.0.0+) installed on your PC.

## Requirements

- [Card Reader Drivers](https://idmelon.com/docs/downloads)
- [WebLogin Extension](https://chromewebstore.google.com/detail/idmelon-weblogin/eagmgpbjpedchliifpgfgogdknnmkaej)
- Chrome or Edge

## Installation

First, install the latest version of the desired reader (AccessKey) and then the latest version of IDmelon WebLogin.

## Autofill Passwords

If you open a login page in a website then tap your card on the reader WebLogin will try to autofill username and
password. In case you haven't added any password before, a dialog will ask you to enter username and password for the
first time. It is also possible to add or manage passwords in [IDmelon Panel](https://panel.idmelon.com/).

## Using Passkeys

WebLogin extension provides capability of creating and using passkeys by tapping your card in a much quicker way than
the platform.

![alt](/images/vendor/weblogin/tap_card.jpg)

Cancelling this dialog will forward request to the platform so user can use other security keys but to disable this
feature completely you can turn off **Handle Passkey Requests** option in extension popup.

![alt](/images/vendor/weblogin/handle_passkey.jpg)

## Workflow Automation

Refer
to [Automation Extension](https://docs.idmelon.com/docs/for_administrators/workflow_automation/automation_extension/)
