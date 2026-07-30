---
title: "Configure Shared Mode with Intune"
description: "Enroll shared Android devices in Intune, install IDmelon Authenticator, and configure Shared Device Mode"
lead: "Enroll shared Android devices in Intune, install IDmelon Authenticator, and configure Shared Device Mode"
date: 2024-07-20T14:40:56+01:00
lastmod: 2026-07-31T00:00:00+00:00
draft: false
images: [ ]
menu:
    docs:
        parent: "android_shared_device"
type: docs
weight: 321100
toc: true
---

This guide covers the whole path an administrator works through: enrolling shared Android devices in **Microsoft
Intune**, force-installing **IDmelon Authenticator** on them, and configuring the app for **Shared Device Mode**.

Shared devices are normally enrolled as **Android Enterprise dedicated devices (COSU)**. They belong to the
organization rather than to a person, so apps and policies are targeted at **device groups**, and only apps assigned as
**Required** ever reach the device.

## Before you start

- Devices running **Android 8.0 or later** with Google Mobile Services (GMS) available. For the enrollment types and
  what each one is for, see Microsoft's
  [Android device enrollment guide for Microsoft Intune](https://learn.microsoft.com/en-us/intune/device-enrollment/android/guide).
- Administrative access to the [Microsoft Intune admin center](https://intune.microsoft.com/).
- Administrative access to the [Microsoft Entra admin center](https://entra.microsoft.com/), if you use MSAL.
- An administrator account for the [IDmelon Panel](https://panel.idmelon.com), with permission to create API keys.
- The hardware for your login method:
  - **Badge** — devices with built-in NFC, or an IDmelon bridge (central hub) to read badges.
  - **Face** — the base URL and API key of your Matcher Server.

> If your shared devices are already enrolled in Android Enterprise, start at
> [Step 4](#step-4--add-idmelon-authenticator-to-intune).

## Step 1 — Connect Intune to Managed Google Play

Without this connection you cannot approve or assign any Android Enterprise app. You do this once per tenant. See
[Connect your Intune account to your Managed Google Play account](https://learn.microsoft.com/en-us/intune/device-enrollment/android/connect-managed-google-play).

## Step 2 — Create the enrollment policy and device group

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com/).

2. Go to **Devices > Enrollment**, select the **Android** tab, and under **Enrollment Profiles** choose
   **Corporate-owned dedicated devices**.

3. Select **Create policy** and enter the basics:

    - **Name:** A name you can recognize later, such as `Shared Android - Front Desk`.

    - **Token type:** Choose the type that matches your deployment:

        - **Corporate-owned dedicated device** — the standard dedicated device token.

        - **Corporate-owned dedicated device with Microsoft Entra ID shared mode** — the same, but Microsoft
          Authenticator is also installed and placed in Microsoft Entra shared device mode during enrollment. Choose
          this one if your deployment uses [MSAL](#signing-in-to-microsoft-apps-with-msal) for single sign-in and
          single sign-out across Microsoft apps.

    - **Token expiration date:** Set a date that fits your rollout and your security policy.

4. Finish the wizard and select **Create**. Intune generates an enrollment token as a 20-digit string and a QR code.
   You can view it later under the policy, using the **Token** option.

5. Create the device group that receives the app and the configuration. A dynamic group keeps itself up to date:

    - Go to **Groups > All groups > New group**.

    - **Group type:** Security. **Membership type:** Dynamic Device.

    - Add the dynamic rule **Property** `enrollmentProfileName` **Equals** the name you gave the enrollment policy.

## Step 3 — Enroll the devices

1. Factory reset each device. This step is required for dedicated device enrollment.

2. Enroll the device with the token from Step 2, using NFC, the QR code, a token string, Google Zero Touch, or Samsung
   Knox Mobile Enrollment. For the steps for each method, see
   [Enroll dedicated, fully managed, or corporate-owned work profile devices](https://learn.microsoft.com/en-us/intune/device-enrollment/android/ref-corporate-methods).

3. Confirm the device appears in **Devices > All devices** and that it landed in the device group from Step 2.

The Microsoft Intune app is installed automatically during enrollment and cannot be removed.

## Step 4 — Add IDmelon Authenticator to Intune

![Adding IDmelon Authenticator as a Managed Google Play app in Intune](/images/vendor/shared_android/create_app.jpg)

1. Go to **Apps > All Apps > Create**.

2. Under **Store app**, select **Managed Google Play app**, then choose **Select**.

3. Search for **IDmelon Authenticator** (`com.vancosys.authenticator.business`) and select it.

4. Approve the app and select **Sync** to sync it into Intune, then select **Refresh** to see it in the app list.

5. Open the app, go to **Properties > Assignments > Edit**, and add your shared device group under **Required**.

> On dedicated devices, only apps assigned as **Required** are installed. An app assigned as *Available* never
> appears, because there is no user to install it from the store.

## Step 5 — Create the Shared Mobile API key

The API key links the app to your IDmelon workspace. Create it before you build the configuration policy.

![Creating a Shared Mobile API key in the IDmelon Panel](/images/vendor/shared_android/get_api_key.jpg)

1. Log in to the IDmelon Panel: [https://panel.idmelon.com](https://panel.idmelon.com).

2. Go to **App Integrations > Authentication > API Key Management**.

3. Click **+ New API Key**.

4. Configure the key settings:

    - **Name:** Enter a recognizable name, for example `Shared Android Devices`.

    - **Type:** Select **Shared Mobile**.

    - **Expiration:** Set an expiration date according to your security policy.

5. Create the key and copy the value.

> Keep this key secure and store it before you close the dialog — it allows the app to authenticate with your
> workspace, and it may not be displayed again. For more information, see
> [API Key Management](/docs/for_administrators/authentication/api_key_management/).

## Step 6 — Create the app configuration policy

![Creating an app configuration policy for managed devices in Intune](/images/vendor/shared_android/create_config.jpg)

1. Go to **Apps > Configuration > Create > Managed devices**.

2. Basics:

   ![Basics page of the app configuration policy](/images/vendor/shared_android/create_config_basics.jpg)

    - **Name:** `IDmelon Android App Configuration`.

    - **Device enrollment type:** Managed devices.

    - **Platform:** Android Enterprise.

    - **Profile Type:** All Profile Types, or the type that matches your deployment.

    - **Targeted app:** Select **IDmelon Authenticator**.

3. Permissions — on the **Settings** page, select **Add** and pre-set the app's runtime permissions so that users are
   not interrupted by prompts on a shared device. Add these three and set each one to **Auto grant**:

    - **Camera**

    - **Post notifications**

    - **Bluetooth connect**

   As of Android 12, Microsoft does not support **Auto grant** for the camera permission on corporate-owned dedicated
   devices, so that prompt still appears on the device. Accept it once during staging, before you hand the device
   over.

4. Settings:

    - Set **Configuration settings format** to **Use configuration designer**.

    - Select **Add** and pick the keys you need. At minimum, select `api_key` and `shared_device_mode`. Add any
      optional keys your deployment requires, for example `shared_login_method`, `shortcut_list`, `auto_logout`,
      `use_msal`, and `azure_client_id`.

    - Enter the values described in the
      [IDmelon Authenticator configuration](#idmelon-authenticator-configuration) section below.

    - If a key does not appear in the designer, switch **Configuration settings format** to **Enter JSON data** and
      supply the same keys there.

5. Assignments — assign the policy to the same **device group** you used for the app in Step 4.

6. Review and create.

> Use literal values rather than Intune's user-based variables (such as user principal name or mail). A dedicated
> device is not associated with a user, so those variables have nothing to resolve to.

## IDmelon Authenticator configuration

To enable shared functionality and link the app to your workspace, you must apply a **Managed Configuration** using the
parameters below. Only `shared_device_mode` and `api_key` are required; the remaining keys are optional and depend on
your deployment.

| Key                    | Type          | Required    | Description                                                                                                                                                                |
|------------------------|---------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `shared_device_mode`   | Boolean       | Yes         | Set to `true`. Enables multi-user optimization and session clearing.                                                                                                       |
| `api_key`              | String        | Yes         | Your unique Shared Mobile API key for workspace authentication. See [Step 5](#step-5--create-the-shared-mobile-api-key).                                                   |
| `shared_login_method`  | String (JSON) | No          | Defines how the shared-device user signs in — by badge or by face. See [Shared login method](#shared-login-method).                                                        |
| `built_in_nfc`         | Boolean       | No          | Whether to use the device's built-in NFC to tap the badge. When `false`, only the bridge (central hub) can be used to read badges.                                         |
| `shortcut_list`        | String (JSON) | No          | App and website shortcuts displayed on the home screen. See [Home screen shortcuts](#home-screen-shortcuts).                                                               |
| `open_url_after_login` | String        | No          | A URL that opens automatically as soon as a user signs in.                                                                                                                 |
| `base_api_url`         | String        | No          | The base API address of your on-premise server.                                                                                                                            |
| `self_service_url`     | String        | No          | Address of the self-service page to open when a tapped badge is not enrolled. See [Self-Service Enrollment Flow](/docs/for_administrators/users_and_security_keys_management/security_key_enrollment_flows/self_service_enrollment_flow/). |
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

Pair this with `built_in_nfc` to control whether the badge is read by the device's own NFC radio or only by the
bridge.

**Face** — the user signs in with face authentication. Provide the connection details of your Matcher Server in
`config`:

```json
{
    "type": "face",
    "config": {
        "base_url": "https://your-matcher-server.com",
        "api_key": "your-matcher-server-api-key"
    }
}
```

- `base_url` — the base URL of your Matcher Server.
- `api_key` — the API key the app uses to authenticate with the Matcher Server. This is **not** the same key as the
  Shared Mobile API key from [Step 5](#step-5--create-the-shared-mobile-api-key).

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

Any app you target with a shortcut must also be installed on the device. Assign it as **Required** to the same device
group, the same way you assigned IDmelon Authenticator in [Step 4](#step-4--add-idmelon-authenticator-to-intune).

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

7. On the registered application, navigate to **Authentication > Add a Redirect URI**, and then continue with the
   remaining steps.

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

## Validate on a test device

Run through this list on one enrolled device before you roll out to the rest of the group.

### In Intune

1. Open **Apps > All Apps > IDmelon Authenticator > Device install status** and confirm the device reports
   **Installed**.
2. Open **Apps > Configuration**, select your policy, and confirm the device is listed as having received it.

### On the device

1. Open IDmelon Authenticator. It should start in shared mode, showing the shared sign-in screen rather than a
   personal setup flow.
2. Sign in with the configured login method — tap a badge, or complete face authentication.
3. Confirm the signed-in identity appears, along with the shortcut tiles from `shortcut_list`.
4. Open a shortcut and confirm the target app or site launches.
5. If `use_msal` is `true`, confirm the Microsoft app opens without a second full sign-in.
6. If `auto_logout` is set, leave the device idle and confirm the session ends after the configured number of minutes.
7. Sign out and confirm the next user starts from a clean session.

## Troubleshooting

| Symptom                                                | What to check                                                                                                                                                                            |
|--------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| The app never installs on the device                   | The assignment must be **Required**, and it must target a **device** group that the device actually belongs to. Confirm the app was synced from Managed Google Play.                       |
| The app installs but does not start in shared mode     | `shared_device_mode` must be Boolean `true`, and the configuration policy must target the same device group as the app.                                                                    |
| The app starts in shared mode but does not activate    | Check the **Shared Mobile** API key: correct type, not expired, and copied without stray whitespace.                                                                                        |
| A permission prompt appears on the shared device       | Pre-set the permission to **Auto grant** in the configuration policy. The camera permission cannot be auto-granted on dedicated devices from Android 12, so accept it once during staging.  |
| Badge taps are not detected                            | If users tap on the device itself, confirm the device has NFC hardware and that `built_in_nfc` is `true`. The bridge (central hub) is always available and needs no key of its own.        |
| Face login fails to reach the Matcher Server           | Verify `base_url` and `api_key` inside `shared_login_method.config`, and that the device network can reach the Matcher Server.                                                              |
| A tapped badge belongs to a user who is not enrolled   | Set `self_service_url` so the device opens your self-service enrollment page instead of failing.                                                                                            |
| Microsoft apps ask for credentials again after sign-in | Verify `use_msal` is `true` and `azure_client_id` matches your own Entra app registration, including the package name and signature hash on the Android platform entry.                     |
| A shortcut tile does nothing                           | The target app must be installed on the device, and the URL scheme must be correct.                                                                                                        |
| Changes to the policy do not reach the device          | Configuration is read when the app starts. Sync the device from Intune, then force-stop and reopen IDmelon Authenticator.                                                                   |
