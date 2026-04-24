---
title: "Microsoft Enterprise SSO profile"
description: "Microsoft Enterprise SSO profile"
lead: ""
date: 2026-04-21T00:00:00-07:00
lastmod: 2026-04-21T00:00:00-07:00
draft: false
images: []
menu:
  docs:
    parent: "shared_ios_ipados_devices_new"
weight: 62500
toc: true
---

This page covers one task only: create the **Microsoft Enterprise SSO** Intune profile required by
the shared iPad MSAL deployment.

## Before you start

Make sure the following items are already in place:

- The **IDmelon Authenticator** app is assigned to the shared iPad device group
- The **Microsoft Authenticator** app is assigned to the same device group
- The managed app configuration from
  [Set up Shared iPad with MSAL](../shared_device_mode_configuration_ios_ipad)
  is already deployed
- The **Microsoft Authenticator** managed app configuration already sets `sharedDeviceMode` to
  Boolean `true`
- The Microsoft Entra app registration described in
  [Microsoft Entra app for MSAL](../configuration_for_using_msal) already exists

## Intune profile

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Go to **Devices > Manage devices > Configuration > Create > New policy**.
3. Set:
   - **Platform**: `iOS/iPadOS`
   - **Profile type**: `Templates > Device features`
4. Click **Create**.
5. In **Basics**, enter a descriptive policy name such as
   `Shared iPad - Microsoft Enterprise SSO`.
6. Click **Next**.

## SSO settings

In **Configuration settings**, enable **Single sign-on app extension** and enter the values below.

### Core values

- **SSO app extension type**: `Redirect`
- **Extension ID**: `com.microsoft.azureauthenticator.ssoextension`
- **Team ID**: Not required for iOS

### URLs

Add the following URLs:

```text
https://login.microsoftonline.com
https://login.microsoft.com
https://sts.windows.net
https://login.partner.microsoftonline.cn
https://login.chinacloudapi.cn
https://login.microsoftonline.us
https://login-us.microsoftonline.com
```

### Extra config

| Key | Type | Value |
| --- | --- | --- |
| `AppPrefixAllowList` | String | `com.idmelon.,com.microsoft.,com.apple.` |
| `browser_sso_interaction_enabled` | Integer | `1` |
| `disable_explicit_app_prompt` | Integer | `1` |

The app prefix allow list should include `com.idmelon.` so the iPad can move cleanly between
IDmelon Authenticator and Microsoft applications in the shared-device flow.

## Assignments

1. Assign the profile to the same device group used for the shared iPad application and managed app
   configuration.
2. Sync one test iPad.
3. Verify that the policy is installed before broad deployment.

## Validation

After the device receives the policy:

1. Open **IDmelon Authenticator** and complete the shared sign-in flow.
2. Open **Teams**, **Outlook**, or the **My Apps** shortcut.
3. Confirm that the Microsoft sign-in flow reuses the existing context instead of repeatedly asking
   for credentials.

If repeated prompts continue, review the Microsoft Enterprise SSO profile, the Entra app
registration, the `use_msal`/`azure_client_id` settings, and the Microsoft Authenticator
`sharedDeviceMode` policy together.
