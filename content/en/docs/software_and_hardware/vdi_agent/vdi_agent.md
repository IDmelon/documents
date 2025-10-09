---
title: "VDI Agent"
description: "Deploying VDI Agent"
lead: ""
date: 2025-10-09T11:07:06+03:30
lastmod: 2025-10-09T11:07:06+03:30
draft: false
images: []
menu:
  docs:
    parent: "vdi_agent"
weight: 20
toc: true
---

**VDI Agent (Virtual Desktop Infrastructure Agent)**<br>
This software enables organizational users to sign in to their virtual desktops using passwordless authentication through the use of a badge card. Instead of entering a username and password, the user simply taps their card on a badge reader, and authentication is performed through **FIDO Security Keys** (Passkeys).
Upon login, the user is automatically inserted into their Remote Desktop session. A second tap of the card triggers an automatic log out.
The application supports both **Windows** and **Linux** platforms.

## Supported Platforms

- Windows 10 and 11
- Linux Ubuntu 22.04 and later
- IGEL OS 12
- Dell Thin OS 2508

## Supported VDI Providers

- Citrix
- VMware Horizon

## SSO Integration with IDmelon

To support passwordless login, you must first integrate with the IDmelon SSO.

[App Integration Guides](https://docs.idmelon.com/docs/for_administrators/app_integrations/integration_guides/app_integration_guides/)

## App Configuration

You can access the app configuration on **Windows** and **Linux (Ubuntu)** platforms through the app menu.

![App Configs](/images/vendor/vdi_agent/app_configs_menu.png)

### VDI Providers

You can select the desired provider from the app menu:

> **Note:** To use each provider, the corresponding software (Citrix Workspace App or Omnisa Horizon Client) must be installed on the device.

![Select Provider](/images/vendor/vdi_agent/select_provider_menu.png)

### Badge Reader Mode

Currently, **rfIDEAS** and **NFC** (PCSC) readers are supported.
If you want to use the reader in **keystroking** mode, enable it from the app menu.

![Keystroking Mode](/images/vendor/vdi_agent/keystroking_menu.png)

### Configurations

- **HTTPS Only:** If your Domain Provider only supports HTTP, you should set `HTTPSOnly` to `False`.
- **Citrix Url:**  The base URL of your Citrix environment.
- **Citrix Path:** Citrix Storefront virtual path (/Citrix/[StoreName]). [For more information](https://developer-docs.citrix.com/en-us/storefront-powershell-sdk/2507/get-stfstoreservice)
- **Citrix Client Version:** The version number of the Citrix Workspace App installed on the device. (Set it to 9909 if your platform is Linux, otherwise specify the actual installed version).
- **VMware Url:**  The base URL of your VMware Horizon environment.
- **VMware Path:** Leave the value as `/`.

### IGEL

App configurations on IGEL OS can be accessed from the following path:

```shell
IGEL Setup > Apps > IDmelon VDI Agent > VDI Agent Settings
```

![App Configs on IGEL](/images/vendor/vdi_agent/igel_setup_configs.png)

## Enabling Tap-to-Login Mode

Enable the **Tap-to-Login** mode from the following path in IDmelon Admin Panel:

```shell
Authentication > Authentication Profile > Managed Authentication Configuration > Badge Tap-to-Login Mode
```

![Enabling Tap-to-Login Mode](/images/vendor/vdi_agent/admin_panel_tap_to_login.png)

## End User Experience

### Tap to Login

When you launch the app, you'll see the IDmelon login page.

![IDmelon Login Page](/images/vendor/vdi_agent/idmelon_login_page.png)

Tap your card on the reader and enter your PIN (You can also set your card verification method to PinLess or use the Remember PIN feature on the Admin Panel to avoid entering the PIN).

![Enter PIN Prompt](/images/vendor/vdi_agent/enter_pin_prompt.png)

### Tap to Logout

If your reader has not configured as `Keystroking Mode`, you can end the current session and disconnect from the remote desktop by tapping your card on the reader for the second time.

You can also end the current session by clicking `Disconnect` button in the app menu.

![Disconnect](/images/vendor/vdi_agent/disconnect_menu.png)
