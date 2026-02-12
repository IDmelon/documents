---
title: "Windows Security Prompt"
description: "Use your FIDO2 security key to provide UAC and ADFS credentials."
lead: ""
date: 2024-09-25T09:00:00+03:30
lastmod: 2024-09-25T09:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "passwords_for_legacy_authentication"
weight: 69000
toc: true
---

In the Windows operating system, the Windows Credential Manager provides a user interface component called Windows Security Prompt which can be used by any service or application to obtain the credentials from the user.
The source which triggers the Windows Security prompt, decides which credential providers are expected to load. It can be the Windows legacy credential provider that takes the username and password from the user or any custom credential provider that deploys custom methods for the authentication.

IDmelon legacy authentication solution with passwords, considers the usage of Windows Security prompt and provides two methods for authentication through this component. These methods are explained bellow:

## Tap-to-login solution with IDmelon password vault

As the Windows Security prompt is a process in Windows that contains credential text boxes, it can be considered as a Windows application; Hence, the tap-to-login solution in \[Windows Applications Environment\]() can be used. Refer to the relevant part in the document for the details.

## IDmelon FIDO Credential Provider (FCP) solution

IDmelon FCP is a custom credential provider that provides Windows Local account and Domain account authentication through FIDO security keys.
If the IDmelon FCP is loaded by the Windows Security prompt to authenticate with domain or Windows local accounts, users will be able to use their FIDO security keys for authentication. In this solution users must have a security key (of any type, i.e., Card, Mobile, or hardware key) registered in IDmelon panel workspace, a Domain or Windows Local password, and have FCP installed on their PC.

The domain account and local account authentication with Windows Security prompt can be used in several scenarios in Windows. Two of the use cases are explained bellow:

### Active Directory Federation Services (ADFS)

The Microsoft on-perm SSO solution that provides user authentication for the applications that do not work with AD is called ADFS. Through this solution, companies will be able to authenticate the user in their own domain and perform a redirection to the third-party applications.

Based on what explained above, if the PC is connected to an Active Directory, you can be authenticated using your domain account credentials and have access to any configured third-party service.

IDmelon FCP provides a domain authentication solution. To deploy this solution, FCP needs to be installed on the PC and a FIDO security key must be registered in IDmelon panel workspace. It can be a contactless card, smartphone or hardware key. Then a Domain type password is required which can be created in IDmelon panel or the onboarding session of FCP.

If the Windows Security prompt loads the IDmelon FCP, user can click on its icon and perform the authentication (which can be by tapping the card on the reader, approving the notification on smartphone, or pushing the button of a hardware key); When authenticated, if the user has a Domain type password, it will be used for the user authentication, and if no password is available, the onboarding session UI will be displayed in the Windows Security prompt from which users can create a domain password immediately.

> **Note:** If the security key owner does not have a domain password but has a Windows local password, FCP automatically tries to authenticate with Windows local password and the onboarding session will not be triggered. To fix this issue in current scenario, you can make sure that no Windows local passwords exist in the user account or you can disable Windows Local authentication in FCP by entering `idmelonfcp advanced-settings --local-authentication disable` command in a command prompt with administrative privileges. It can be enabled later by `idmelonfcp advanced-settings --local-authentication enable` command.

### User Account Control (UAC)

In Windows standard accounts where administrative privileges are not granted, when trying to open an application as administrator, a Windows Security prompt comes up which loads IDmelon FCP. In this case, if you have a domain password or a Windows local password in your IDmelon panel account, and you also have a security key registered in IDmelon panel, you are be able to authenticate through your security key and run that application as administrator.

> **Note:** If the PC is connected to an Active Directory, domain passwords are prioritized if available.
>
> **Note:** If you enable the user verification of a password, you’ll need be verified by your authenticator (security key) in order to be authenticated (like entering the PIN of the security key).

## Step by step guide

1. Install IDmelon FCP.
2. Register a security key in IDmelon panel.
3. If the security key is a contactless card make sure you have IDmelon card reader driver installed.
4. Go through your desired authentication procedure; If the Windows Security prompt comes up and loads the IDmelon FCP, perform an action on your security key (like tapping the card on the reader), then fill in the credentials of the domain or Windows local account in the onboarding session. Or you can create the required password directly from IDmelon panel.
5. Go through the authentication procedure again and you will be able to authenticate with your FIDO security key without need of entering your credentials.
