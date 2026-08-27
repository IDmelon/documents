---
title: "IDmelon Agent"
description: "Deploying IDmelon Agent"
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

**IDmelon Agent**<br>
IDmelon Agent delivers simple, fast, and secure sign-ins to virtual desktops and cloud apps using employee badges, smartphones, biometrics, or security keys. It automates sign-in, session launch, and user switching for Microsoft, Citrix, and VMware environments.

The agent supports two modes:

- **VDI Mode:** For virtual desktop environments such as Citrix and Horizon.
- **Kiosk Mode:** For Microsoft Entra ID sign-in to cloud apps, followed by opening the configured URL after successful authentication. Ideal for shared, multi-user environments such as retail, healthcare, manufacturing, and education, where Tap-to-Login and Tap-to-Logout keep sessions secure and prevent them from being left open.

## Supported Platforms

- Windows 10 and 11
- Linux Ubuntu 22.04 and later
- IGEL OS 12
- Dell ThinOS 2508

## App Configuration

On Windows and Linux, open the app menu and select **Configuration**.

### Agent Mode

Use **Agent Mode** to choose how the device will be used.

Changing the mode is possible from agent settings and the drop-down menu in configuration. This restarts the app so the new flow can load.

![Agent Mode - App Settings](/images/vendor/vdi_agent/IDmelon_Agent/select_agent_mode_1.png)
![Agent Mode - Configuration](/images/vendor/vdi_agent/IDmelon_Agent/select_agent_mode_2.png)

### Badge Reader Mode

The agent can read badges directly from PC/SC NFC and rfIDEAS readers. If your reader sends badge IDs as keyboard input, enable **Keystroking Mode** from the app menu.

If the badge ID is read in the wrong order, enable **Reverse Byte Order**.

![Badge Reader Mode](/images/vendor/vdi_agent/IDmelon_Agent/keystroking_menu.png)

### Shared Configuration Fields

- **Workspace ID:** Optional. Use this when requests should be scoped to a specific IDmelon workspace.
- **API Key:** Optional. Required only when your workspace enforces authenticated clients (Admin Panel > Authentication > API Key Management (API Key Type: Clients)).
- **Base URL:** Optional. Use this for on-premise environments.
- **Self-Service URL:** Optional. The self-service enrollment portal opened in-app when a tapped badge is not enrolled yet. See [Self-Service Badge Enrollment](#self-service-badge-enrollment).

## Self-Service Badge Enrollment

When a user taps a badge that is not enrolled yet, the agent can open your self-service enrollment portal in an in-app window. The user enrolls the badge on the spot and then taps again to sign in.

This works the same way in VDI Mode and Kiosk Mode.

For background on the enrollment flow itself, see [Self-Service Enrollment Flow](/docs/for_administrators/users_and_security_keys_management/security_key_enrollment_flows/self_service_enrollment_flow/).

The **Self-Service URL** in the agent must be configured for this to work.

### Agent Configuration

First, get the address of your self-service enrollment page from the IDmelon Admin Panel.

On the IDmelon Admin Panel, navigate to **Security keys** and select the blue button named **Workflows** on the top-right side. Choose **Self-Service actions**, and copy the generated URL:

```shell
Security keys > Workflows > Self-Service actions
```

![Self-Service URL-1](/images/vendor/vdi_agent/IDmelon_Agent/self_service_url_1.png)
![Self-Service URL-2](/images/vendor/vdi_agent/IDmelon_Agent/self_service_url_2.png)
![Self-Service URL-3](/images/vendor/vdi_agent/IDmelon_Agent/self_service_url_3.png)

Then set **Self-Service URL** to that address.

- On Windows and Linux, open the app menu, select **Configuration**, and fill in **Self-Service URL**.
- On IGEL OS, set it from `IGEL Setup > Apps > IDmelon Agent > Agent Settings > Self-Service URL`.

This field is optional. If you leave it empty, an unenrolled badge shows the **This badge is not enrolled.** error as before.

### Self-Service Enrollment User Experience

1. The user taps a badge that is not enrolled.
2. The agent opens the self-service portal in an in-app window titled **Enroll your badge**.
3. The user completes the enrollment steps on the portal.
4. The user selects **Back to Home** to close the enrollment window. This also works as a cancel, if the user decides not to enroll.
5. The agent returns to the tap screen. The user taps the badge again to sign in.

> **Note:** Badge taps are ignored while the enrollment window is open.

## VDI Mode

Use **VDI Mode** when the device should launch a virtual desktop session after the user taps their badge.

![App Configs](/images/vendor/vdi_agent/IDmelon_Agent/app_configs_menu_VDI.png)

### Supported VDI Providers

- Citrix
- Omnissa Horizon (VMware Horizon)

> **Note:** To use a VDI provider, install the matching client on the device first: Citrix Workspace App for Citrix, or Omnissa Horizon Client for Horizon.

### VDI Configuration

Select the provider from the app menu:

- **Citrix**
- **Horizon**

![Select Provider](/images/vendor/vdi_agent/IDmelon_Agent/select_provider_menu.png)

Configure the provider settings:

- **HTTPS Only:** Set to `False` only if your provider environment uses HTTP or a self-signed certificate.
- **Citrix URL:** The base URL of your Citrix environment.
- **Citrix Path:** The Citrix StoreFront virtual path, such as `/Citrix/[StoreName]`. [For more information](https://developer-docs.citrix.com/en-us/storefront-powershell-sdk/2507/get-stfstoreservice)
- **Citrix Client Version:** The Citrix Workspace App version installed on the device. Use `9909` on Linux.
- **VMware URL:** The base URL of your Horizon environment.
- **VMware Path:** Leave this value as `/`.

### VDI User Experience

When the app opens in VDI Mode, the user sees the tap screen.

![Tap Card Screen](/images/vendor/vdi_agent/IDmelon_Agent/tap_card_screen.png)

The user taps their badge on the reader. If a PIN is required, the agent asks for it before continuing.

![Enter PIN Screen](/images/vendor/vdi_agent/IDmelon_Agent/enter_pin_screen.png)

After the badge is verified, the agent completes the passkey sign-in flow and launches the configured Citrix or Horizon desktop.

![Loading Screen - Verifying Credentials](/images/vendor/vdi_agent/IDmelon_Agent/loading_screen_1.png)
![Loading Screen - Establishing secure session](/images/vendor/vdi_agent/IDmelon_Agent/loading_screen_2.png)
![Loading Screen - Preparing Workspace](/images/vendor/vdi_agent/IDmelon_Agent/loading_screen_3.png)

After the remote session is active, the user can tap the badge again to disconnect from the session.

Alternatively, use **Disconnect** from the app menu.

![Disconnect Option](/images/vendor/vdi_agent/IDmelon_Agent/disconnect_option.png)

### Auto-Launch for a Single Desktop

On Omnissa Horizon, if the signed-in user is entitled to exactly one desktop, the agent starts that desktop directly instead of leaving the user on the Horizon client's desktop list.

If the user is entitled to more than one desktop, the Horizon client opens its desktop list as usual and the user picks one.

This behavior is automatic and requires no configuration. It applies to Horizon only; Citrix sessions are not affected.

### SSO Integration with IDmelon

To support passwordless login, first integrate your VDI provider with IDmelon SSO.

[App Integration Guides](https://docs.idmelon.com/docs/for_administrators/app_integrations/integration_guides/app_integration_guides/)

### Enabling Passkey Login mode

Enable **Default Provider** and **Auto-Start Login with Passkey** from the IDmelon Admin Panel:

```shell
Authentication > Authentication Profile > Managed Authentication Configuration > Default Provider
```

```shell
Authentication > Authentication Profile > Managed Authentication Configuration > Auto-Start Login with Passkey
```

![Enabling Tap-to-Login Mode](/images/vendor/vdi_agent/IDmelon_Agent/admin_panel.png)

## Kiosk Mode

Use **Kiosk Mode** when the device is shared by multiple users and should open a browser-based workspace after badge sign-in. The Kiosk URL does not control the authentication flow. The agent always uses the Microsoft Entra ID login process first, then loads the configured URL after successful authentication.

![App Configs](/images/vendor/vdi_agent/IDmelon_Agent/app_configs_menu_Kiosk.png)

### Supported Platforms

- Microsoft Entra ID login using passkeys
- Microsoft apps such as MyApps, Outlook, and Teams
- Any other website that uses Microsoft SSO login.

### Kiosk Configuration

- **Kiosk URL:** The URL to open after successful Microsoft Entra ID authentication. This URL is the post-login destination, not the login provider.

### Kiosk User Experience

In Kiosk Mode, the user follows the same tap card, enter PIN, and loading process as VDI Mode. The agent signs the user in through the Microsoft Entra ID login process with IDmelon passkey authentication, and after authentication succeeds, it loads the configured Kiosk URL.

When the user is finished, they tap the badge again or select **End Session** from the app menu. The agent clears the browser session and returns to the tap screen for the next user.

![End Session Option](/images/vendor/vdi_agent/IDmelon_Agent/end_session_option.png)

## IGEL

On IGEL OS, settings are managed from the IGEL profile instead of the local app menu.

Open the configuration from:

```shell
IGEL Setup > Apps > IDmelon Agent > Agent Settings
```

![App Configs on IGEL](/images/vendor/vdi_agent/IDmelon_Agent/igel_setup_configs_VDI.png)
![App Configs on IGEL](/images/vendor/vdi_agent/IDmelon_Agent/igel_setup_configs_Kiosk.png)

The app menu on IGEL is read-only for settings. It shows the current mode, provider, and reader state, and still allows users to disconnect or exit.
