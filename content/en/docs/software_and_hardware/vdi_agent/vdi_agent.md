---
title: "IDmelon Agent"
description: "Deploying IDmelon Agent"
lead: ""
date: 2025-10-09T11:07:06+03:30
lastmod: 2026-10-27T11:07:06+03:30
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

### Reader Settings

The agent reads badges directly from PC/SC NFC and rfIDEAS readers. On Windows, if IDmelon Accesskey is installed and running, the agent also receives badge taps from Accesskey. No configuration is needed.

Open the app menu and select **Reader**:

- **Card Type:**
  - **Default:** The badge ID is used to sign in. Use this for regular employee badges.
  - **FIDO:** The card itself signs in. Use this for NFC FIDO security keys, such as Crescendo cards, held on a PC/SC reader. The user enters the card's own PIN.
- **Keystroking Mode > Enabled:** Turn this on if your reader sends badge IDs as keyboard input. Not available when **Card Type** is **FIDO**.
- **Keystroking Mode > Reverse Byte Order:** Turn this on if the badge ID is read in the wrong order. Available only when keystroking is on.

Changing the card type restarts the app.

![Reader Menu](/images/vendor/vdi_agent/IDmelon_Agent/reader_menu.png)

### Shared Configuration Fields

- **Workspace ID:** Optional. Use this when requests should be scoped to a specific IDmelon workspace.
- **API Key:** Optional. Required only when your workspace enforces authenticated clients (Admin Panel > Authentication > API Key Management (API Key Type: Clients)).
- **Base URL:** Optional. Use this for on-premise environments.
- **Self-Service URL:** Optional. The self-service enrollment portal opened in-app when a tapped badge is not enrolled yet. See [Self-Service Badge Enrollment](#self-service-badge-enrollment).

### Collect Logs

To send logs to support, open the app menu and select **Collect Logs**. Choose a folder and select **Save Here**. The agent saves all its logs there as one zip file.

If the agent cannot write to the chosen folder, it saves the zip file in its own log folder and shows where.

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

## Forced PIN Change

If an administrator requires a PIN change for a badge or security key, the agent asks for a new PIN before signing in. This works the same way in VDI Mode and Kiosk Mode.

1. The user taps the badge.
2. The agent shows **Change your PIN** with the message **Your PIN must be changed before you can sign in.**
3. The user enters **Current PIN**, **New PIN**, and **Confirm new PIN**, and selects **Change PIN**.

To stop without changing the PIN, the user selects **Cancel**. The agent returns to the tap screen.

The new PIN must:

- Meet the length shown under the fields, for example **Use 4 to 16 characters.**
- Be different from the current PIN.
- Not start or end with a space.

![Change PIN Screen](/images/vendor/vdi_agent/IDmelon_Agent/change_pin_screen.png)

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
- **Citrix Config:**
  - **URL:** The base URL of your Citrix environment.
  - **StoreFront Path:** The Citrix StoreFront web path, such as `/Citrix/StoreWeb`. [For more information](https://developer-docs.citrix.com/en-us/storefront-powershell-sdk/2507/get-stfstoreservice)
  - **Client Version:** The Citrix Workspace App version installed on the device. Use `9909` on Linux and IGEL.
- **Omnissa Horizon Config:**
  - **URL:** The base URL of your Horizon environment.
  - **Path:** Leave this value as `/`.

The agent supports both the classic and the new Citrix StoreFront login experience. No configuration is needed.

### Progress Overlay View

By default, the agent's overlay stays on screen through the whole sign-in, until the virtual desktop opens.

To show only the tap and PIN screens, open the app menu and clear **Progress Overlay View**. The overlay then steps aside after the PIN, and comes back when the session ends.

On IGEL OS, set it from `IGEL Setup > Apps > IDmelon Agent > Agent Settings > Progress Overlay View`.

![Progress Overlay View](/images/vendor/vdi_agent/IDmelon_Agent/progress_overlay_menu.png)

### Citrix End Session

Choose what a second tap, or **Disconnect** in the app menu, does to a Citrix session:

- **Disconnect Only:** The session is disconnected and keeps running on the server. The user can reconnect to it later.
- **Disconnect and Log Off:** The session is disconnected and the user is logged off.

On Windows and Linux, open the app menu and select **Provider > Citrix End Session**. The option is shown only when **Citrix** is the selected provider.

On IGEL OS, set it from `IGEL Setup > Apps > IDmelon Agent > Agent Settings > Citrix End Session`.

![Citrix End Session](/images/vendor/vdi_agent/IDmelon_Agent/citrix_end_session_menu.png)

### VDI User Experience

When the app opens in VDI Mode, the user sees the tap screen.

![Tap Card Screen](/images/vendor/vdi_agent/IDmelon_Agent/tap_card_screen.png)

The user taps their badge on the reader. If a PIN is required, the agent asks for it before continuing.

![Enter PIN Screen](/images/vendor/vdi_agent/IDmelon_Agent/enter_pin_screen.png)

After the badge is verified, the agent completes the passkey sign-in flow and launches the configured Citrix or Horizon desktop.

![Loading Screen - Verifying Credentials](/images/vendor/vdi_agent/IDmelon_Agent/loading_screen_1.png)
![Loading Screen - Establishing secure session](/images/vendor/vdi_agent/IDmelon_Agent/loading_screen_2.png)
![Loading Screen - Preparing Workspace](/images/vendor/vdi_agent/IDmelon_Agent/loading_screen_3.png)

After the remote session is active, the user can tap the badge again to end the session. On Citrix, this disconnects or logs off, depending on [Citrix End Session](#citrix-end-session). If an application in the virtual desktop is listening for badge taps through the IDmelon VDI Bridge, the tap can go to that application instead. See [Badge Taps in the Virtual Desktop](#badge-taps-in-the-virtual-desktop).

Alternatively, use **Disconnect** from the app menu.

![Disconnect Option](/images/vendor/vdi_agent/IDmelon_Agent/disconnect_option.png)

### Auto-Launch for a Single Desktop

On Omnissa Horizon, if the signed-in user is entitled to exactly one desktop, the agent starts that desktop directly instead of leaving the user on the Horizon client's desktop list.

If the user is entitled to more than one desktop, the Horizon client opens its desktop list as usual and the user picks one.

This behavior is automatic and requires no configuration. It applies to Horizon only; Citrix sessions are not affected.

### Badge Taps in the Virtual Desktop

The IDmelon VDI Bridge is installed together with the agent on Windows, Linux, and IGEL OS. It carries badge taps from the endpoint into the Citrix or Horizon session, so applications running inside the virtual desktop can react to them.

To receive taps, the virtual desktop must run the **IDmelon VDI Session** app, and at least one application in the session must be connected to it. See [Deploy VDI Session](/docs/software_and_hardware/vdi_agent/deploy_vdi_session/).

After a successful sign-in, the agent waits for the session to start. As soon as an application in the session is connected to the IDmelon VDI Session app, the agent sends the badge used for sign-in into the session once.

When the user taps a badge while the session is active:

- **An application in the session is connected:** The tap is sent into the session. The application must confirm within 3 seconds whether it handled the tap.
  - **Handled:** The session stays open.
  - **Not handled, or no answer within 3 seconds:** The session ends, as with a normal second tap.
- **Card Type is FIDO:** The tap is sent into the session and the session stays open. No confirmation is needed.
- **No application is connected, or the VDI Bridge is unavailable:** The tap ends the session, as in earlier versions.

The confirmation is an event sent from the session to the endpoint, so the VDI Bridge must accept session events. Otherwise every confirmed tap ends the session. On IGEL OS, this is **Accept Session Events**, which is on by default.

> **Note:** If the VDI Bridge is not installed or not reachable, the agent works as before. Sign-in and the second-tap disconnect are not affected.

### Citrix Policies

On Citrix, set these policies in Citrix Studio (or Web Studio) for the delivery groups that serve your virtual desktops.

#### Allow the IDmelon Virtual Channel

Badge taps reach the virtual desktop over a custom Citrix virtual channel named `IDMLN`. On the endpoint, the VDI Bridge installer registers the `IDMLN` driver in Citrix Workspace App. In the virtual desktop, Citrix blocks the channel until it is allowed:

1. In Citrix Studio, open the **Virtual channel allow list** policy setting (**ICA > Security**).
2. Add this entry:

   ```text
   IDMLN,C:\Program Files\IDmelon\VDI Session\IDmelonVDISession.exe
   ```

3. Restart the virtual desktops. They read the allow list when they start.

This is a one-time, site-wide change. Without it, the IDmelon VDI Session app cannot open the channel and badge taps do not reach the session.

#### Keep the Security Key Prompt Inside the Session

Keep the security key prompt inside the virtual desktop, instead of sending it to the endpoint:

1. In Citrix Studio, search the policy settings for `FIDO2` and `WebAuthn`.
2. Set **FIDO2 redirection** to **Prohibited**.
3. Set **WebAuthn redirection** to **Prohibited** too, if it is listed.
4. Reconnect the session.

![Citrix FIDO2 Redirection Policy](/images/vendor/vdi_agent/IDmelon_Agent/citrix_fido2_policy.png)

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

The settings are grouped as follows:

- **General:** **Agent Mode**, **VDI Provider**, **Https Only**, and **Progress Overlay View** in VDI Mode, or **Kiosk Url** in Kiosk Mode.
- **Citrix Config:** **Citrix Url**, **Citrix StoreFront Path**, **Client Version**, and **Citrix End Session**.
- **Omnissa Horizon Config:** **Omnissa Horizon Url** and **Omnissa Horizon Path**.
- **Workspace:** **Workspace ID**, **API Key**, **Base URL**, and **Self-Service URL**.
- **Reader:** **Keystroking Mode**, **Reverse Card Bytes**, and **Card Type**.
- **VDI Bridge** (VDI Mode only):
  - **Shared Secret:** Must match the shared secret of the IDmelon VDI Session app in the virtual desktop. Badge taps and session events are encrypted and signed with it. If it is empty, they are sent unencrypted and unsigned. Use a hex string of at least 32 characters, for example the output of `python -c "import secrets; print(secrets.token_hex(32))"`, and set the same value as `sharedSecret` in the VDI Bridge config on Windows and Linux endpoints (`C:\ProgramData\IDmelon\VDI Bridge\config.json` or `~/.config/idmelon/vdibridge/config.json`) and in the IDmelon VDI Session config in the virtual desktop (`C:\ProgramData\IDmelon\VDI Session\config.json`).
  - **Accept Session Events:** Accept events that applications in the virtual desktop send to the device, such as the tap confirmation. On by default.
  - **Max Clock Skew (seconds):** Session events older or newer than this are rejected.

![VDI Bridge Settings on IGEL](/images/vendor/vdi_agent/IDmelon_Agent/igel_setup_vdi_bridge.png)

The app menu on IGEL is read-only for settings. It shows the current mode, provider, Citrix End Session, keystroking, card type, and Progress Overlay View, and still allows users to disconnect, collect logs, or exit.
