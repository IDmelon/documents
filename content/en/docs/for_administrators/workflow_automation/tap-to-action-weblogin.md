---
title: "Tap to action with WebLogin"
description: ""
lead: ""
date: 2024-07-17T10:30:55+03:30
lastmod: 2024-07-17T10:30:55+03:30
draft: false
images: [ ]
menu:
  docs:
    parent: "workflow_automation"
weight: 96200
toc: true
---

The WebLogin is an extension that can be installed on PC along with **IDmelon AccessKey** to automate the user login and
logout routine in shared environments. Therefore, by tapping the user's card on the reader, the login steps are done
automatically, as well as for the user to exit the shared environment.

## Requirements

- [Card Reader Drivers](https://idmelon.com/docs/downloads)
- [WebLogin Extension](https://chromewebstore.google.com/detail/idmelon-weblogin/eagmgpbjpedchliifpgfgogdknnmkaej)
- Chrome or Edge

## Installation

First, install the latest version of the desired reader (AccessKey) and then the latest version of WebLogin extension.

## Enable Workflow Automation

To switch to automation mode, you must first enter the following command in a PowerShell.

```shell
accesskeycli workflow-automation -s -t weblogin-extension -a login-logout -u https://myapps.microsoft.com?login_hint=${UserId} -m passkey
```

- `-s`: To activate automation and configure it.
- `-t weblogin-extension`: This value is always fixed.
- `-a login-logout`: To set the automation actions. In this case, both the login and logout procedures can be
  automated. (Other possible actions: `-a login` , `-a logout`)
- `-u https://myapps.microsoft.com?login_hint=${UserId}`: To set the target URL. In this case, we set the Microsoft 365
  URL.
- `-m passkey`: Method of automation which can be passkey or password

Open the following link in Chrome/Edge and enable **Allow in Incognito**/**Allow in InPrivate** option:
> chrome://extensions/?id=eagmgpbjpedchliifpgfgogdknnmkaej

## Automation

After the workflow automation setup has been finished, a pinned tab will be opened in the browser.

![alt](/images/vendor/workflow_automation/weblogin/pinned_tab.png)

When the user taps the card on the reader, the automation will be started and clicks will be done automatically.
If the user has already logged in and the card is tapped again, the private window will close automatically and be ready
for the next user to log in.

## Disable Workflow Automation

To disable the Workflow Automation, enter the following command in a PowerShell.

```shell
accesskeycli workflow-automation -r
```
