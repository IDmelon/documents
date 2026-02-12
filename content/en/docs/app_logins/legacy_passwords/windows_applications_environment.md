---
title: "Windows Applications Environment"
description: "Log in to Windows applications using your FIDO2 security key."
lead: ""
date: 2024-09-25T09:00:00+03:30
lastmod: 2024-09-25T09:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "passwords_for_legacy_authentication"
weight: 68000
toc: true
---

In Windows applications environment, users are able to use the **tap-to-login** solution for any process or application with credential forms, by the use of IDmelon card reader driver and a Windows Application Password which is assigned to the card owner in corresponding workspace in IDmelon panel.

This solution comes with the card reader driver as a feature named Password Vault which must be enabled by entering `accesskeycli password-vault --enable` in a command prompt with administrative privileges. It can be disabled later by `accesskeycli password-vault --disable` command.

Once the password vault is enabled, when a card is tapped and the owner of the card has a password for the focused application, it filles the credentials into the credential boxes if the cursor is active in a text box. The password vault first fills the username, virtually clicks the TAB key, then fills the password and finally clicks the ENTER key virtually.

The Windows Application passwords can be assigned to a user directly from the IDmelon panel workspace or users can add the password to their account through the onboarding prompt of the password vault immediately after tapping the card. In order to enable the onboarding prompt for an application, the application name (the process name of the window containing the credential forms) must be added to the onboarding allow list in password vault by entering `accesskeycli password-vault --onboarding-allowlist --add application_name_1, application_name_2` command in a command prompt with administrative privileges. The application names can be removed later by `accesskeycli password-vault --onboarding-allowlist --remove application_name_1, application_name_2` command.

When adding a Windows Application password in IDmelon panel or when enabling the onboarding prompt for an application, the process name of the window which contains the credential text boxes is a requirement. This process name can be obtained using a scanner tool which comes with the card reader driver. The scanner tool can be executed by `accesskeycli scanner` command; When running, it prints the process name of the focused window.

### Step by step guide

1. Install IDmelon Card reader driver
2. Enable password vault by `accesskeycli password-vault --enable`.
3. Run application name scanner by `accesskeycli scanner`.
4. Click on the window of the application which contains the credential forms (username and password text boxes).
5. Then click on the scanner tool and copy what it has recognized as the application name.
6. Enable the onboarding by `accesskeycli password-vault --onboarding-allowlist --add [copied-application-name]` then click on the application window and tap the card to add the password in your PC; Or you can login to the IDmelon panel and add a new Windows Application password in the Passwords tab using the copied application name.
7. Click on the application window and tap the card, then the credentials will be filled automatically.
   Note: When adding a password in the IDmelon Admin Panel, ensure the application name matches exactly with what was recognized by the scanner tool.

> **Note:** Each password can require user verification to be filled. When the user verification is enabled for a password, the card PIN will be asked before filling the credentials.
>
> **Note:** To enable user verification for a password, either toggle the corresponding button in the IDmelon Admin Panel or enter the security key PIN in the PC onboarding prompt.
