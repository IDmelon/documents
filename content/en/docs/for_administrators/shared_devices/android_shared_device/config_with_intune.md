---
title: "Configure Shared Mode with Intune"
description: ""
lead: ""
date: 2024-07-20T14:40:56+01:00
lastmod: 2024-07-20T14:40:56+01:00
draft: false
images: [ ]
menu:
    docs:
        parent: "android_shared_device"
type: docs
weight: 321100
toc: true
---

This document provides instructions for IT administrators to deploy and configure the **IDmelon Authenticator** app on
Android Enterprise devices using **Microsoft Intune**, specifically optimized for **Shared Device Mode**.

## Prerequisites

- A Microsoft Intune license and administrative access to
  the [Microsoft Intune admin center](https://intune.microsoft.com/).
- Android Enterprise enrolled devices (Dedicated, Fully Managed, or Corporate-owned with Work Profile).
- The IDmelon Authenticator app added to your Managed Google Play Store.
- An active administrator account for the [IDmelon Panel](https://panel.idmelon.com).

## Generating the API Key

Before configuring Intune, you must generate a unique API key from the IDmelon Panel to link the mobile app to your
organization.

![alt](/images/vendor/shared_android/get_api_key.jpg)

1. Log in to the IDmelon Panel: [https://panel.idmelon.com](https://panel.idmelon.com).

2. Navigate to **Authentication** > **API Key Management** in the left-hand sidebar.

3. Click the **+ New API Key** button.

4. Configure the key settings:

    - **Name:** Enter a recognizable name (e.g., "Shared Android Devices").

    - **Type:** Select Shared Mobile.

    - **Expiration:** Set an appropriate expiration date according to your security policy.

5. Click Save/Generate and copy the API Key string. You will need this for the Intune configuration in the next step.

> Keep this key secure. It allows the app to authenticate with your workspace.

## Configuration Details

To enable shared functionality and link the app to your workspace, you must apply a **Managed Configuration** using the
parameters below. Only `shared_device_mode` and `api_key` are required; the remaining keys are optional and depend on
your deployment.

| Key                    | Type          | Required    | Description                                                                                                                                                                |
|------------------------|---------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `shared_device_mode`   | Boolean       | Yes         | Set to `true`. Enables multi-user optimization and session clearing.                                                                                                       |
| `api_key`              | String        | Yes         | Your unique Shared Mobile API key for workspace authentication. See [Generating the API Key](#generating-the-api-key).                                                     |
| `shared_login_method`  | String (JSON) | No          | Defines how the shared-device user signs in — by badge or by face. See [Shared login method](#shared-login-method).                                                        |
| `built_in_nfc`         | Boolean       | No          | Whether to use the device's built-in NFC to tap the badge. When `false`, only the bridge (central hub) can be used to read badges.                                         |
| `shortcut_list`        | String (JSON) | No          | App and website shortcuts displayed on the home screen. See [Home screen shortcuts](#home-screen-shortcuts).                                                               |
| `open_url_after_login` | String        | No          | A URL that opens automatically as soon as a user signs in.                                                                                                                 |
| `base_api_url`         | String        | No          | The base API address of your on-premise server.                                                                                                                            |
| `self_service_url`     | String        | No          | Address of the self-service page to open when a tapped badge is not enrolled.                                                                                              |
| `auto_logout`          | Integer       | No          | Automatically signs the user out after a fixed time, in minutes (e.g., `60` signs the user out after one hour).                                                            |
| `use_msal`             | Boolean       | No          | Uses MSAL to sign the user in to Microsoft apps immediately after they sign in to IDmelon Authenticator. See [MSAL setup](#signing-in-to-microsoft-apps-with-msal).        |
| `azure_client_id`      | String        | Conditional | Required when `use_msal` is `true`. The **Application (client) ID** from your Microsoft Entra app registration. See [MSAL setup](#signing-in-to-microsoft-apps-with-msal). |

### Shared login method

The `shared_login_method` key defines how a user signs in on the shared device. Two methods are supported.

**Badge** — the user signs in by tapping their badge:

```json
{
    "type": "badge"
}
```

**Face** — the user signs in with face authentication. Provide the base URL of your face authentication service in
`config.base_url`:

```json
{
    "type": "face",
    "config": {
        "base_url": "https://your-face-service.com"
    }
}
```

### Home screen shortcuts

The `shortcut_list` key displays app and website shortcuts on the home screen. Each shortcut can point to a website or
an
Android native application through its URL scheme. Provide a JSON array of objects, each with `iconName`, `title`, and
`url`:

```json
[
    {
        "iconName": "teams",
        "title": "Teams",
        "url": "msteams://"
    },
    {
        "iconName": "github",
        "title": "Github",
        "url": "https://github.com"
    }
]
```

- `iconName` — the icon shown for the shortcut.
- `title` — the label shown under the icon.
- `url` — a website URL (for example, `https://github.com`) or an Android app URL scheme (for example, `msteams://`).

## Signing in to Microsoft apps with MSAL

Set `use_msal` to `true` to sign users in to Microsoft apps (such as Teams and Outlook) automatically, right after they
sign in to IDmelon Authenticator.

When `use_msal` is `true`, `azure_client_id` is **mandatory**. This value is the **Application (client) ID** of a
Microsoft Entra app registration that you create for IDmelon Authenticator. The next section walks through creating it.

### Registering the Android app in Microsoft Entra

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com/).

2. If you manage more than one tenant, switch to the tenant used by your shared device deployment.

3. Go to **Entra ID > App registrations** and click **New registration**.

4. Enter a recognizable name such as `IDmelon-Authenticator-Shared-Android`.

5. Under **Supported account types**, select the option that matches your tenant policy. In most deployments, this is
   **Accounts in this organizational directory only**.

6. Click **Register**.

7. Open the new app registration and go to **Authentication > Add a platform**.

8. Select **Android** and enter the following values:

    - **Package name:** `com.vancosys.authenticator.business`

    - **Signature hash:** `w6OyxDHFH36OaPPikqYmsRzWAFo=`

9. Click **Configure**. Microsoft Entra generates the redirect URI for the app automatically.

10. Go to **API permissions** and confirm that the delegated **User.Read** permission for **Microsoft Graph** is
    present.
    If it is missing, click **Add a permission > Microsoft Graph > Delegated permissions**, select **User.Read**, and
    add
    it.

11. Click **Grant admin consent** for the tenant if your policy requires it.

12. From the **Overview** page, copy the **Application (client) ID**.

Place the copied **Application (client) ID** into the `azure_client_id` key in your managed app configuration, and set
`use_msal` to `true`. Use the client ID from your own Entra app registration — do not reuse the example value.

## Step-by-Step Setup Instructions

1. Add IDmelon Authenticator to Intune

   ![alt](/images/vendor/shared_android/create_app.jpg)

    1. Navigate to Apps > Android > Create.

    2. Select Managed Google Play app as the App type.

    3. Search for IDmelon Authenticator (com.vancosys.authenticator.business).

    4. Approve and sync the app to your Intune environment.

    5. Click on Refresh, Select the app and assign it as Required for desired users/groups.

2. Create the App Configuration Policy

   ![alt](/images/vendor/shared_android/create_config.jpg)

    1. Go to Apps > Android > Configuration > Create > Managed devices.

    2. Basics:

       ![alt](/images/vendor/shared_android/create_config_basics.jpg)

        - Name: IDmelon Android App Configuration.

        - Platform: Android Enterprise.

        - Profile Type: All Profile Types (or specific to your deployment).

        - Targeted App: Select IDmelon Authenticator.

    3. Settings:

        - Set Configuration settings format to Use configuration designer.

        - Click Add to pick the keys you need. At minimum, select `api_key` and `shared_device_mode`. Add any optional
          keys from the table above that your deployment requires (for example, `shared_login_method`, `shortcut_list`,
          `auto_logout`, `use_msal`, and `azure_client_id`).

        - Enter the values provided in the table above.

3. Deployment (Assignments)

    1. Under the Assignments tab, include the Device Groups that represent your shared hardware.

    2. Review and Create.
