---
title: "Home Screen and Kiosk Experience"
description: "Choose how a shared Android device presents its apps, and set up multi-app kiosk mode with Managed Home Screen"
lead: "Choose how a shared Android device presents its apps, and set up multi-app kiosk mode with Managed Home Screen"
date: 2026-07-31T00:00:00+03:30
lastmod: 2026-07-31T00:00:00+03:30
draft: false
images: []
menu:
    docs:
        parent: "android_shared_device"
type: docs
weight: 321300
toc: true
---

> **This page is optional.** A shared device works without any of it. Locking the home screen down is a matter of
> organization preference — how much of the device you want users to reach, and how tightly it must be controlled.
> If you have no such requirement, stop after
> [Deploy and Configure IDmelon Authenticator](/docs/for_administrators/shared_devices/android_shared_device/config_with_intune/).

If you do want that control, a dedicated device can present its apps in more than one way. This page covers the choice
and walks through the setup that suits shared devices: **multi-app kiosk mode** with Microsoft's **Managed Home
Screen** launcher.

## Before you start

- Devices enrolled as **Android Enterprise dedicated devices** running **Android 8.0 or later**. See
  [Enroll Shared Devices in Intune](/docs/for_administrators/shared_devices/android_shared_device/enroll_devices_in_intune/).
- IDmelon Authenticator deployed and configured. See
  [Deploy and Configure IDmelon Authenticator](/docs/for_administrators/shared_devices/android_shared_device/config_with_intune/).
- The list of apps users are allowed to open on the device.

## Choose the experience

| Option | What the user sees | Use it when |
|--------|--------------------|-------------|
| **No kiosk lockdown** | The device's normal launcher, with every installed app | You have no lockdown requirement, or you are still piloting the deployment |
| **Multi-app kiosk** | Managed Home Screen, showing only the apps you list | The usual choice for a shared device that also needs Teams, Outlook, or a browser |
| **Single-app kiosk** | IDmelon Authenticator only, full screen, with no way out | Rare. Only when websites opened from inside IDmelon are the entire job |

**Multi-app kiosk** is the option most deployments want. Every app a shortcut tile points at must also be in the kiosk
app list, otherwise the tile appears but does nothing.

**Single-app kiosk** is far more restrictive than it first sounds. The device runs IDmelon Authenticator and nothing
else: no other native apps, and no separate web browser. That leaves exactly one workable pattern — pin IDmelon
Authenticator as the kiosk app and publish everything the user needs as website shortcuts in `shortcut_list`, opened
from inside IDmelon. Shortcuts pointing at native apps such as `msteams://` cannot open under this mode. It fits
single-purpose stations and little else.

## Set up multi-app kiosk mode

In this mode, **Managed Home Screen** replaces the device launcher, and the apps you list run on top of it.

### Step 1 — Install the apps the kiosk will show

Every app that appears in the kiosk — including Managed Home Screen itself — must be added in Intune and assigned as
**Required** to your shared device group.

1. Go to **Apps > Android > Create** and add **Managed Home Screen** as a **Managed Google Play app**, then sync it.

2. Assign it as **Required** to the shared device group.

3. Confirm the same is true for **IDmelon Authenticator** and for every app your `shortcut_list` opens, such as Teams
   or Outlook.

### Step 2 — Create the device restrictions profile

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com/).

2. Go to **Devices > Manage devices > Configuration > Create > New policy**.

3. Set **Platform** to **Android Enterprise**, and under **Fully Managed, Dedicated, and Corporate-Owned Work
   Profile** choose **Device restrictions**. Select **Create**.

4. Give the policy a name such as `Shared Android - Kiosk`, then continue to **Configuration settings**.

5. Open **Device experience** and set:

    - **Device experience type:** **Kiosk mode (dedicated and fully managed)**.

    - **Kiosk mode:** **Multi-app**.

    - **Add:** select each app that belongs on the home screen — Managed Home Screen, IDmelon Authenticator, and every
      shortcut target.

   The app list also has an **Allow without sign in** column. It only takes effect if you turn on the Managed Home
   Screen sign-in screen in Step 3, and it does nothing on its own. Keep the app list here rather than in the Managed
   Home Screen app configuration policy — both write the same list, and configuring it in two places leads to
   conflicts.

6. Set **Leave kiosk mode** to **Enable** and set a **Leave kiosk mode code** — a 4 to 6 digit PIN. This is how an
   administrator temporarily leaves the kiosk to service the device. Without it, a locked device is hard to work on.

7. Assign the policy to the same **device group** used for the apps, then review and create.

To leave the kiosk on a device, press the back button repeatedly until **Exit kiosk** appears, enter the PIN, and open
**Managed Home Screen** again when you are done to relock the device.

### Step 3 — Choose how users reach the apps

Managed Home Screen can either show the apps immediately, or ask the user to sign in to the home screen first. Both are
configured in an app configuration policy for **Managed Home Screen**:

1. Go to **Apps > Configuration > Create > Managed devices**.

2. Set **Platform** to **Android Enterprise** and choose **Managed Home Screen** as the targeted app.

3. Configure the mode you want, using the configuration designer or the JSON editor.

4. Assign the policy to the shared device group.

#### Mode 1 — without Managed Home Screen sign-in

Leave **Enable sign in** off. This is the default.

The home screen shows IDmelon Authenticator and the other apps as soon as the device starts. The user signs in through
IDmelon Authenticator with a badge tap or face authentication, and if you set `use_msal`, IDmelon then signs them in to
the Microsoft apps on the device. IDmelon remains the only place a user signs in.

#### Mode 2 — with Managed Home Screen sign-in

Set **Enable sign in** to `true`. Managed Home Screen then shows its own sign-in screen and keeps the apps hidden until
the user signs in.

On a shared device this needs one more thing, because the user has to reach IDmelon Authenticator *before* they can
sign in anywhere:

- In this policy, set **Sign in type** to **Microsoft Entra ID**, so that signing in to Managed Home Screen also signs
  the user in to the other apps on the device that participate in Microsoft Entra shared device mode.

- Back in the **device restrictions** profile from Step 2, tick **Allow without sign in** for **IDmelon
  Authenticator** in the kiosk app list. In the Managed Home Screen JSON this is the same as
  `app_available_prior_to_sign_in: true` inside that app's entry in the `applications` array, next to its `package`
  key — but set it in one place only, not both.

The user opens IDmelon Authenticator from the sign-in screen, authenticates with a badge or face, uses it to sign in
to Managed Home Screen, and the apps appear.

Three things to expect in this mode:

- Apps allowed before sign-in are **not shown as tiles**. They open from an entry point on the **top bar** of the
  sign-in screen.
- Users must sign in again after every device reboot.
- If your device restrictions profile enables the **Home and Overview buttons**, or shows system notifications in the
  status bar, users can skip past the sign-in screen.

### Step 4 — Customize Managed Home Screen (optional)

The same app configuration policy carries the rest of the Managed Home Screen settings — grid size, wallpaper, screen
saver, organization logo, session PIN, and automatic sign-out. For the full list, see Microsoft's
[Configure the Microsoft Managed Home Screen app](https://learn.microsoft.com/en-us/intune/app-management/configuration/configure-managed-home-screen).

## Validate on a test device

1. Sync the test device and wait for the policies to arrive.
2. Confirm the device starts into Managed Home Screen rather than the stock launcher.
3. In Mode 2, confirm the sign-in screen appears and that IDmelon Authenticator can be opened from it.
4. Sign in through IDmelon Authenticator with a badge or face.
5. Confirm IDmelon Authenticator and every allowed app appear, and that nothing else is reachable.
6. Open each shortcut tile and confirm its target launches.
7. Confirm the **Exit kiosk** PIN works for administrators.

## Troubleshooting

| Symptom | What to check |
|---------|---------------|
| The device still shows the stock launcher | Managed Home Screen must be assigned as **Required** to the device group, and the device restrictions policy must target the same group. |
| An app is missing from the kiosk | The app must be both assigned as **Required** and listed under **Device experience > Add**. Doing only one of the two is the usual cause. |
| Users reach the sign-in screen but cannot open IDmelon Authenticator | In Mode 2, IDmelon Authenticator must have **Allow without sign in** ticked. Without it there is no way to sign in at all. |
| **Allow without sign in** is ticked but no apps are reachable before sign-in | Three usual causes: **Enable sign in** is not `true` in the Managed Home Screen app configuration policy, so there is no sign-in screen for it to apply to; the apps are behind the **top bar** entry point rather than shown as tiles; or the app list is configured in both the device restrictions profile and the app configuration policy, and the two conflict. |
| Signing in to Managed Home Screen does not sign the user in to other apps | **Sign in type** must be **Microsoft Entra ID**, and the other apps must participate in Microsoft Entra shared device mode. |
| Users skip past the Managed Home Screen sign-in screen | Check the **Enabled System Navigation Features** and **System notifications and information** settings in the device restrictions profile. |
| A shortcut tile opens nothing | The target app is not in the kiosk app list, or its URL scheme is wrong. Under single-app kiosk, only website shortcuts can open. |
| Administrators cannot service the device | **Leave kiosk mode** must be enabled and a PIN set before the device is locked down. |

## Reference

- [Device restriction settings for Android in Microsoft Intune](https://learn.microsoft.com/en-us/intune/device-configuration/templates/ref-device-restrictions-android-enterprise)
- [Configure the Microsoft Managed Home Screen app](https://learn.microsoft.com/en-us/intune/app-management/configuration/configure-managed-home-screen)
