---
title: "Set up Shared iPad with MSAL"
description: "Set up Shared iPad with MSAL"
lead: "Set up Shared iPad with MSAL"
date: 2026-04-21T00:00:00-07:00
lastmod: 2026-04-21T00:00:00-07:00
draft: false
images: []
menu:
  docs:
    parent: "shared_ios_ipados_devices_new"
weight: 62100
toc: true
---

This is the main setup guide for the current **Shared iPad with MSAL** deployment.

The goal is simple:

- Deploy **IDmelon Authenticator** to shared iPads
- Apply the shared-mode configuration
- Put **Microsoft Authenticator** in shared mode
- Connect the iPad to **Microsoft Entra ID** through MSAL
- Let users open **Teams**, **Outlook**, and **My Apps** after they sign in

This guide is intentionally shorter than the older shared iPad documentation. It focuses only on
the deployment you are using now.

## What you are setting up

- **Shared mode** in IDmelon Authenticator
- **Shared mode** in Microsoft Authenticator
- **MSAL** for Microsoft applications
- **Shortcut tiles** such as Teams, Outlook, Google, and My Apps
- **Self-service fallback** for users who are not fully enrolled yet

## Before you start

- iOS or iPadOS **17 or later**
- Administrative access to the [Microsoft Intune admin center](https://intune.microsoft.com)
- Administrative access to the [Microsoft Entra admin center](https://entra.microsoft.com)
- Access to the [IDmelon Admin Panel](https://panel.idmelon.com)
- The **IDmelon Authenticator** app available in your MDM catalog
- The **Microsoft Authenticator** app available in your MDM catalog
- A **Shared Mobile** API key created in the IDmelon Admin Panel
- Permission to create an app registration in Microsoft Entra ID

## API key

Before you configure the iPad, create a workspace API key dedicated to shared mobile devices.

1. Open the [IDmelon Admin Panel](https://panel.idmelon.com).
2. Go to **App Integrations > Authentication > API Key Management**.
3. Click **+ New API Key**.
4. Enter a descriptive name such as `Shared iPad - Intune`.
5. Set the **Type** acceto **Shared Mobile**.
6. Create the key and copy the value securely.

For more information, see [API Key Management](/docs/for_administrators/authentication/api_key_management/).

## Entra app

Before you build the Intune policy, create the Microsoft Entra app registration and copy the
**Application (client) ID**.

Use this reference page:

- [Microsoft Entra app for MSAL](../configuration_for_using_msal)

You will place that client ID into the `azure_client_id` setting.

## Intune app

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Go to **Apps > iOS/iPadOS**.
3. Add **IDmelon Authenticator** as an iOS store app.
4. Assign the app to the shared iPad device group.

![Intune app add flow for IDmelon Authenticator](/images/vendor/shared_ipads_new/intune_panel_apps_idmelon.png)

Assign **Microsoft Authenticator** to the same shared iPad device group. The MSAL flow depends on
Microsoft Authenticator being present on the device.

## App configuration

Create a managed app configuration policy for **IDmelon Authenticator** and apply the following
sanitized `dict` payload.

```xml
<dict>
  <key>api_key</key>
  <string>YOUR_SHARED_MOBILE_API_KEY</string>
  <key>azure_client_id</key>
  <string>YOUR_ENTRA_APP_CLIENT_ID</string>
  <key>shared_authentication_backend</key>
  <string>legacy</string>
  <key>shared_device_passkeys</key>
  <true/>
  <key>shared_login_method</key>
  <dict>
    <key>type</key>
    <string>face</string>
    <key>model</key>
    <string>hid</string>
  </dict>
  <key>shortcut_list</key>
  <array>
    <dict>
      <key>iconName</key>
      <string>teams</string>
      <key>title</key>
      <string>Teams</string>
      <key>url</key>
      <string>msteams://</string>
    </dict>
    <dict>
      <key>iconName</key>
      <string>outlook</string>
      <key>title</key>
      <string>Outlook</string>
      <key>url</key>
      <string>ms-outlook://</string>
    </dict>
    <dict>
      <key>iconName</key>
      <string>google.com</string>
      <key>title</key>
      <string>Google</string>
      <key>url</key>
      <string>https://google.com</string>
    </dict>
    <dict>
      <key>iconName</key>
      <string>myapps</string>
      <key>title</key>
      <string>My Apps</string>
      <key>url</key>
      <string>https://myapps.microsoft.com/?login_hint={email}</string>
    </dict>
  </array>
  <key>use_msal</key>
  <true/>
  <key>self_service_url</key>
  <string>https://panel.idmelon.com/self-service/YOUR_SELF_SERVICE_WORKFLOW_ID</string>
</dict>
```

Use your own values for:

- `api_key`
- `azure_client_id`
- `self_service_url`

Keep these values exactly as shown unless your deployment team gives you a different requirement:

- `shared_authentication_backend` = `legacy`
- `shared_device_passkeys` = `true`
- `use_msal` = `true`

## Shared login method

Set `shared_login_method` explicitly in the managed app configuration. The deployment can use one
of these patterns:

### Face login

Use this when the shared iPad sign-in flow should use face-based authentication hardware:

```xml
<key>shared_login_method</key>
<dict>
  <key>type</key>
  <string>face</string>
  <key>model</key>
  <string>hid</string>
</dict>
```

- `type = face` means the user signs in with the face-based method
- `model = hid` means the configured HID-connected reader model is used

### Badge login

Use this when the shared iPad sign-in flow should use badge tap instead:

```xml
<key>shared_login_method</key>
<dict>
  <key>type</key>
  <string>badge</string>
  <key>model</key>
  <string>auto</string>
</dict>
```

- `type = badge` means the user starts sign-in by tapping their badge
- `model = auto` means the app automatically detects the supported badge-reader setup

Choose the option that matches your hardware and sign-in flow. Do not leave
`shared_login_method` undefined if your deployment depends on a specific login method.

The shortcut list in the current setup includes:

- **Teams**
- **Outlook**
- **Google**
- **My Apps**

If you use a different shortcut set in production, update only that array.

## Microsoft Authenticator

Create a separate managed app configuration policy for **Microsoft Authenticator** and apply this
required setting:

| Key | Type | Value |
| --- | --- | --- |
| `sharedDeviceMode` | Boolean | `true` |

This setting must be applied to **Microsoft Authenticator**, not to **IDmelon Authenticator**.
Without it, the shared iPad MSAL sign-in flow will not behave like a shared device deployment.

Use the same device group used for the shared iPad rollout so both apps receive their policies
before testing.

## Enterprise SSO

After both app configurations are ready, configure the Microsoft Enterprise SSO Intune profile.

Use this reference page:

- [Microsoft Enterprise SSO profile](../configuration_for_microsoft_enterprise_sso)

## Test sign-in

1. Open **IDmelon Authenticator** on the shared iPad.
2. Complete the configured shared login method.
3. Open **Teams**, **Outlook**, or **My Apps**.
4. Confirm the device has already received the **Microsoft Authenticator** policy with
   `sharedDeviceMode = true`.
5. When the Microsoft dialog appears, continue with the MSAL sign-in flow.
6. When the passkey prompt appears, use the passkey.

![Microsoft sign-in dialog](/images/vendor/shared_ipads_new/msal_face_fingerprint_pin_or_security_key.png)
![Passkey prompt](/images/vendor/shared_ipads_new/msal_use_passkey_prompt.png)

## After sign-in

After a successful sign-in, the user should return to the shared-mode home screen and see:

- the signed-in identity at the top of the page
- the available shortcut tiles
- Microsoft apps ready to open without repeating the full sign-in flow

![Shared mode home screen after login](/images/vendor/shared_ipads_new/shared_ipad_home_after_login.png)

## Troubleshooting

- If Microsoft apps still ask for repeated credentials, verify both `use_msal` and the Enterprise
  SSO profile.
- If the Microsoft flow does not act like a shared-device session, verify the **Microsoft
  Authenticator** policy sets `sharedDeviceMode` to Boolean `true`.
- If the app does not activate automatically, verify the **Shared Mobile** API key.
- If **My Apps** does not open correctly, confirm that the shortcut URL still contains
  `login_hint={email}`.
- If unenrolled users do not get redirected correctly, verify the `self_service_url`.
