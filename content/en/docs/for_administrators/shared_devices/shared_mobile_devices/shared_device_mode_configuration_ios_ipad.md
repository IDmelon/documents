---
title: "Shared Device Mode Configuration Using Intune"
description: ""
lead: ""
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

Shared mobile devices are widely used across many industries, including manufacturing and retail. IDmelon Authenticator can be configured in shared device mode, enabling users to use their badge or biometric factors to load passkeys or passwords into the device instantly. This allows users to access multiple applications with auto-login. The session time-to-live can be configured to support various shift change use cases with auto logout.

**IDmelon Authenticator Shared Configuration Mode Features:**

- **Passkeys Autofill**
- **MSAL Configuration** to streamline access to Microsoft apps
- **SSO Integration**

Microsoft Intune is a cloud-based endpoint management solution. It manages user access to organizational resources and simplifies app and device management across your many devices, including mobile devices, desktop computers, and virtual endpoints.

## Prerequisites

- To use the shared iPad feature, the OS version of iPads must be **17 or later**.
- Microsoft Authenticator (for login via MSAL).

## How to Configure

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
6. Select **iOS 17.0** as the **Minimum operation system** and click the **Next** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_app_minimum_os.png)
7. Configure the **Assignments** tab according to your organization's policies.
![MS Intune](/images/vendor/shared_ipads/intune_panel_app_assignments.png)
8. Review the configurations you set and click the **Create** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_app_review.png)
9. Navigate to **Apps > App configuration policies**, click the **+ Add**, and then **Managed devices**.
![MS Intune](/images/vendor/shared_ipads/intune_panel_configuration_policies.png)
10. Set a **Name**, select the **iOS/iPadOS** as the Platform, and **IDmelon Authenticator** as the **Targeted app**, then click the **Next** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_configuration_policies_app.png)
11. In the **Settings** tab, select the **User configuration designer** as the **Configuration settings format**, Enter values for the XML property list as below, and click the **Next** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_configuration_policies_configurtion_keys.png)

| Configuration key      | Value type  | Configuration value  |
|------------------------|-------------|----------------------|
| shared_device_passkeys | Boolean     | true                 |
| authentication_type    | String      | onInit               |
| device_id              | String      | {{deviceid}}         |
| api_key                | String      | [API_KEY]            |

<br>

**One-Time Use Passkeys:**

Add the following key to the above configurations to automatically log the user out after their first login with the passkey.

| Configuration key      | Value type  | Configuration value  |
|------------------------|-------------|----------------------|
| one_time_use_passkeys  | Boolean     | true                 |

<br>

**Dedicated Deployments Base API URL:**

If you are using dedicated deployment, such as on-premises, add the following key-value to the configurations.

| Configuration key      | Value type  | Configuration value         |
|------------------------|-------------|-----------------------------|
| base_api_url           | String      | https://example.com/api/url |

<br>

**Self Service URL:**

By adding the self-service URL (available in the [IDmelon Admin Panel](https://panel.idmelon.com) > Security Keys > Workflows > Self-Service Actions) to the configurations, users will be redirected to the enrollment page if their card is not yet registered.

| Configuration key      | Value type  | Configuration value                          |
|------------------------|-------------|----------------------------------------------|
| self_service_url       | String      | https://panel.idmelon.com/self-service/{UID} |

<br>

**Auto Logout:**

By adding this configuration, users will be automatically logged out of the app after a specified time or after the first use of the passkey.<br>
*Allowed values: one-time use, 5m, 60m, 2h, 4h, 6h, 8h*

| Configuration key      | Value type  | Configuration value         |
|------------------------|-------------|-----------------------------|
| auto_logout            | String      | 2h                          |

<br>

**Allowed values for authentication_type:**

    **onInit**: User is required to enter PIN once only after tapping the card on a reader.
    **onUse**: User is required to enter PIN for each login.
    **none**: The login process is done without entering any PIN (PINless mode).

- Configure the **Assignments** tab according to your organization's policies, and click the **Next** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_configuration_policies_assignments.png)
- Review the configurations you set and click the **Create** button.
![MS Intune](/images/vendor/shared_ipads/intune_panel_configuration_policies_review.png)

### Configure MSAL Login

The configuration below is required to enable login through MSAL.

Refer to [How to configure](#how-to-configure) - step 9 for instructions on configuring the app.

| Configuration key      | Value type  | Configuration value        |
|------------------------|-------------|----------------------------|
| shared_device_passkeys | Boolean     | true                       |
| shared_device_mode     | Boolean     | true                       |
| azure_client_id        | String      | [Application (client) ID]  |
| api_key                | String      | [API_KEY]                  |

### Generate a New API Key

The API Key is required to activate IDmellon Authenticator automatically when it is run for the first time. Therefore, there would be no need for manual activation to connect to the organization.<br>
To generate a new API Key, take the following steps:

- Sign in to the [IDmelon Admin Panel](https://panel.idmelon.com).
- Navigate to **Authentication > API Key Management**.
- Select **Create**.
- Set a name.
- Set the type to **Shared Mobile**.
- Copy and replace the generated key with the **API_KEY** value in the app configuration.

## End User Experience

1. Open the **IDmelon Authenticator**.
2. Get close to the reader (or plug the keystroke reader) and tap your card on it.
3. According to the **Card Verification Method** set in the IDmelon admin panel, if PIN is required, enter it. Otherwise, go to the next step.
![Authenticator](/images/vendor/shared_ipads/shared_ipad_enter_pin.PNG)
4. In case of successful login, user information will be displayed. At this stage, move the app to the background.
![Authenticator](/images/vendor/shared_ipads/shared_ipad_logged_in.PNG)
5. Open the app you want to log in with the passkey (e.g. MS Teams), tap the paste icon on the top left corner of the keyboard, and tap the **Next**.
![MS Teams](/images/vendor/shared_ipads/msteams_login_page.PNG)
6. Tap the **Next**.
![MS Teams](/images/vendor/shared_ipads/msteams_login_with_passkey.PNG)
7. Tap the **Continue**.
![MS Teams](/images/vendor/shared_ipads/shared_ipad_msteams_passkey.PNG)

### MSAL Login Experience

If you use Microsoft applications (MS Teams, Excel, PowerBI, etc …), the **Microsoft Authenticator** app must be added and configured as shared mode.

[Config MS Authenticator as shared device mode](https://www.petervanderwoude.nl/post/getting-started-with-shared-device-mode-for-ios-devices/#Configuring-the-Microsoft-Authenticator-app-for-iOS-devices-with-shared-device-mode)

- Complete the user login steps (1-4) in the IDmelon Authenticator app according to the previous section.
- Once the user logs in, go through the MSAL prompts by selecting the Continue and Next buttons.
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_1.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_2.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_3.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_4.PNG)

- Move the app to the background.
- Open any app you want to sign in to (for example, Teams).
- Paste the email using the paste icon on the keyboard.

### Logout Experience

1. Open the app you logged into, and log out of the account.
2. Open the **IDmelon Authenticator** and then tap the logout icon on the top right corner of the app. Your user information and existing passkeys will be deleted from the iPad.
![Authenticator](/images/vendor/shared_ipads/shared_ipad_logout.PNG)

## Shared Device Mode Configuration From the App Settings

To manually adjust some of the shared iPad's configs, do it from the following path:<br>
**App Menu > Security Key Settings > App Configs**.

![App Configs](/images/vendor/shared_ipads/shared_ipad_app_configs.png)

**Auto-Logout:**

By enabling this config, users will be automatically logged out of the app after a specified time or after the first use of the passkey.<br>

**Redirect After Login:**

By enabling this config, users will be redirected to a specific app or URL after logging in. (e.g., msteams:// will open the Microsoft Teams app if it is available on the device).

**Redirect After Logout:**

Similar to "Redirect After Login," after users log out, they will be redirected to the desired app or address.

**Require Authentication:**

- **On Init**: Users must enter a PIN once after tapping the card on the reader.<br>
- **On Use**: Users are required to enter a PIN for each login.<br>
- **None**: The login process is completed without entering any PIN (PINless mode).<br><br>

***Note**: The value of the **Require Authentication** depends on the **Card Verification Method** that your organization's admins have set in the IDmelon admin panel.*
