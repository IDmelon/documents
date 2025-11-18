---
title: "Set up an iOS or iPadOS device in Shared Device Mode"
description: "Set up an iOS or iPadOS device in Shared Device Mode"
lead: "Set up an iOS or iPadOS device in Shared Device Mode"
date: 2023-09-20T15:19:33+03:30
lastmod: 2023-09-20T15:19:33+03:30
draft: false
images: []
menu:
  docs:
    parent: "shared_mobile_devices"
weight: 62100
toc: true
---

Shared mobile devices are widely used across many industries, including manufacturing and retail. IDmelon Authenticator can be configured in shared device mode, enabling users to use their badge or biometric authentication to load passkeys into the device instantly. This allows users to access multiple applications with auto-login. The session time-to-live can be configured to support various shift change use cases with auto logout.

IDmelon Authenticator Shared Configuration Mode Features:

- **Passkeys Autofill:** Provides passkeys to the operating system for authentication.
- **SSO Integration:** Enables quick access to IDmelon-connected service providers.
- **MSAL Integration:** Streamlines access to Microsoft apps.

This configuration is tested with Microsoft Intune as MDM but can work with other MDMs as well to push configuration.

---

## Prerequisites

- iOS or iPadOS **17 or later**.

---

## Configure IDmelon Authenticator

Use these managed app configuration keys to set up **IDmelon Authenticator** in **shared device mode** via your MDM (e.g., Microsoft Intune, Jamf, Kandji, VMware Workspace ONE). Push them as standard managed app settings to enrolled devices.

> **Scope:** Controls shared-mode behavior, PIN prompts, self-service flows, shortcuts, and optional MSAL shared-mode integration.

### Configuration Keys

| Key                      | Type        |    Required | Purpose                                                                 |
| ------------------------ | ----------- | ----------- | ----------------------------------------------------------------------- |
| `shared_device_passkeys` | Boolean     |         Yes | Enable shared device mode (`true`).                                     |
| `authentication_type`    | String      |         Yes | When to prompt for PIN (`onInit`, `onUse`, `none`).                     |
| `device_id`              | String      |         Yes | MDM device identifier (e.g., Intune `{{deviceid}}`).                    |
| `api_key`                | String      | Recommended | Auto-activate app (Admin Panel → **Shared Mobile** API key).            |
| `one_time_use_passkeys`  | Boolean     |          No | Remove/invalidate passkey after first successful login.                 |
| `base_api_url`           | String      |          No | Target dedicated/on-prem API endpoint.                                  |
| `self_service_url`       | String      |          No | Redirect to self-service enrollment if card isn’t registered.           |
| `auto_logout`            | String      |          No | Scheduled logout (`one-time use`, `5m`, `60m`, `2h`, `4h`, `6h`, `8h`). |
| `shortcut_list`          | JSON string |          No | In-app shortcuts (titles, URLs, icons).                                 |
| `shared_device_mode`     | Boolean     |          No | MSAL shared device mode flag (requires MS Authenticator shared mode).   |
| `azure_client_id`        | String      |          No | Azure App Registration Client ID for MSAL.                              |
| `open_url_after_login`   | String      |          No | Launches a specified app or URL immediately after the user signs in.    |
| `open_url_after_logout`  | String      |          No | Launches a specified app or URL immediately after the user logs out.    |

### Key Details & Valid Values

#### `shared_device_passkeys` (Boolean, **Required**)

Enable shared device mode.

```json
"shared_device_passkeys": true
```

#### `authentication_type` (String, **Required**)

Controls PIN prompts:

- `onInit` — PIN once after first tap
- `onUse` — PIN every login
- `none` — PINless login

```json
"authentication_type": "onInit"
```

#### `device_id` (String, **Required**)

MDM device identifier used for device binding.

- **Intune:** `{{deviceid}}`
- **Others:** use the provider's device identifier token

```json
"device_id": "{{deviceid}}"
```

#### `api_key` (String, **Recommended**)

Auto-activates the app on first run (no manual activation). Generate under **Admin Panel → Authentication → API Key Management → Create → Type: Shared Mobile**.

```json
"api_key": "YOUR_GENERATED_KEY"
```

#### `one_time_use_passkeys` (Boolean, Optional)

Logs user out after the first successful login with the passkey.

```json
"one_time_use_passkeys": true
```

#### `base_api_url` (String, Optional)

Specify this when pointing to a dedicated/on-premises API endpoint.

```json
"base_api_url": "https://your-dedicated-server.example.com/api"
```

#### `self_service_url` (String, Optional)

Redirect users to self-service enrollment if their card isn’t registered (Admin Panel → **Security Keys → Workflows → Self-Service Actions**).

```json
"self_service_url": "https://panel.idmelon.com/self-service"
```

#### `auto_logout` (String, Optional)

Auto-logout after the given duration or after single use.
**Allowed:** `one-time use`, `5m`, `60m`, `2h`, `4h`, `6h`, `8h`

```json
"auto_logout": "2h"
```

#### `shortcut_list` (JSON string, Optional)

Quick-access shortcuts inside the app.

**Pretty:**

```batch
{
  \"shortcuts\": [
    { \"title\": \"My Apps\", \"url\": \"https://myapps.microsoft.com\", \"iconName\": \"microsoft.com\" },
    { \"title\": \"MS Teams\", \"url\": \"https://teams.microsoft.com\", \"iconName\": \"Teams\" }
  ]
}
```

> The `\` before the `"` is required.

#### `open_url_after_login`, `open_url_after_logout` (String, Optional)

Immediately after a user successfully signs in (or out), the app launches a specified application or web address.

```json
"open_url_after_login": "https://..."
```

> Don't use these configurations if you have configured MSAL

It can be a URL scheme (to open a native app directly) or a regular URL (to open a website in the browser).

**Examples for URL Scheme:**

- Launch Microsoft Teams: `msteams://`
- Launch Microsoft Excel: `ms-excel://`

```json
"open_url_after_login": "app-scheme://"
```

#### `shared_device_mode` (Boolean, Optional)

If you plan to use Microsoft applications (such as Teams, Outlook, and other Office apps) on shared iPads, we recommend using **MSAL**.

This and the next configuration are **required** when enabling MSAL.

> For detailed steps on configuring **MSAL**, please refer to [Configuration for using MSAL](../configuration_for_using_msal).

```json
"shared_device_mode": true
```

#### `azure_client_id` (String, Optional)

Azure App Registration **Client ID** for MSAL.

```json
"azure_client_id": "YOUR_AZURE_APP_CLIENT_ID"
```

## Configure IDmelon Authenticator using Intune

1. Navigate to **Apps > iOS/iPadOS**.
![MS Intune](/images/vendor/shared_ipads/intune_panel_apps.png)
2. Click the **Add** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_apps_add.png)
3. Select **iOS store app** as the **App type** and click the **Select** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_apps_search.png)
4. Click the **Search the App Store**.
![MS Intune](/images/vendor/shared_ipads/intune_panel_apps_search_appstore.png)
5. Search for the **IDmelon Authenticator** app and click the **Select** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_apps_idmelon.png)
6. Select **iOS 17.0** as the **Minimum operating system** and click the **Next** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_app_minimum_os.png)
7. Configure the **Assignments** tab according to your organization's policies.
![MS Intune](/images/vendor/shared_ipads/intune_panel_app_assignments.png)
8. Review the configurations you set and click the **Create** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_app_review.png)
9. Navigate to **Apps > App configuration policies**, click the **+ Add**, and then **Managed devices**.
![MS Intune](/images/vendor/shared_ipads/intune_panel_configuration_policies.png)
10. Set a **Name**, select the **iOS/iPadOS** as the Platform, and **IDmelon Authenticator** as the **Targeted app**, then click the **Next** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_configuration_policies_app.png)
11. In the **Settings** tab, select the **User configuration designer** as the **Configuration settings format**, enter values for the configuration keys as explained in the [Configuration Keys](#configuration-keys) section above, and click the **Next** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_configuration_policies_configurtion_keys.png)

## Login Experience

When IDmelon Authenticator is configured in shared mode, users can walk in, tap their badge, and get the passkeys available to the device for authentication. Depending on the configuration, you can provide multiple user experiences.

### Example 1: Basic configuration

Use the following configuration object when your MDM asks for app configuration (exact UI varies).

```json
{
  "shared_device_passkeys": true,
  "authentication_type": "onInit",
  "device_id": "{{deviceid}}",
  "api_key": "YOUR_API_KEY"
}
```

> **Note**: If you are using Microsoft products on the shared iPad (such as Microsoft Teams, Outlook, or Microsoft 365), we recommend configuring the **Microsoft Enterprise SSO plug-in**.
This plug-in simplifies authentication and reduces repeated credential prompts (see [Configuring the Microsoft Enterprise SSO plug-in](../configuration_for_using_msal/#configuring-the-microsoft-enterprise-sso-plug-in)).

1. Open the **IDmelon Authenticator**.
2. Get close to the reader (or plug the keystroke reader) and tap your card on it.
3. According to the **Card Verification Method** set in the IDmelon admin panel, if PIN is required, enter it. Otherwise, go to the next step.
![Authenticator](/images/vendor/shared_ipads/shared_ipad_enter_pin.PNG)
4. In case of successful login, user information will be displayed. At this stage, move the app to the background.
![Authenticator](/images/vendor/shared_ipads/shared_ipad_logged_in.PNG)
5. Open the app you want to log in to using the passkey (e.g., MS Teams), tap the paste icon on the top left corner of the keyboard, and tap **Next**.
![MS Teams](/images/vendor/shared_ipads/msteams_login_page.PNG)
6. Tap the **Next**.
![MS Teams](/images/vendor/shared_ipads/msteams_login_with_passkey.PNG)
7. Tap the **Continue**.
![MS Teams](/images/vendor/shared_ipads/shared_ipad_msteams_passkey.PNG)

### Example 2: With MSAL Login Experience

Once **MSAL** has been configured (see [configuration guide](../configuration_for_using_msal)), you can leverage the integrated authentication experience across Microsoft applications.

Use the following configuration object when your MDM asks for app configuration (exact UI varies).

```json
{
  "shared_device_passkeys": true,
  "authentication_type": "onInit",
  "device_id": "{{deviceid}}",
  "api_key": "YOUR_API_KEY",
  "shared_device_mode": true,
  "azure_client_id": "YOUR_AZURE_APP_CLIENT_ID"
}
```

- Complete the user login steps (1-4) in the IDmelon Authenticator app according to the previous section.
- Once the user logs in, go through the MSAL prompts by selecting the Continue and Next buttons.
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_1.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_2.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_3.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_4.PNG)

- Move the app to the background.
- Open any app you want to sign in to (for example, Teams).
- Paste the email using the paste icon on the keyboard.

## Logout Experience

There are several ways to log out. One is manual, which you can see below. Another way is by managing the temporary session on iPad, which can streamline the logout experience. For more information, see [Temporary Session Experience on Shared iPad](../temporary_session_experience).

### Manual Logout from IDmelon Authenticator

This is a manual logout that, when MSAL is configured, can also trigger sign-out from Microsoft connected services as well.

1. Open the app you logged into, and log out of the account.
2. Open the **IDmelon Authenticator** and then tap the logout icon on the top right corner of the app. Your user information and existing passkeys will be deleted from the iPad.
![Authenticator](/images/vendor/shared_ipads/shared_ipad_logout.PNG)
