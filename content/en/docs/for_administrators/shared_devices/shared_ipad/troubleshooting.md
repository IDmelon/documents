---
title: "Troubleshooting"
description: "Symptom, cause, and fix for common shared-iPad setup problems."
lead: "When something does not work."
date: 2026-06-30T00:00:00-07:00
lastmod: 2026-06-30T00:00:00-07:00
draft: false
images: []
menu:
  docs:
    parent: "shared_ipad"
weight: 62360
toc: true
---

Find your symptom, apply the fix. Each assumes you already pushed a managed app
configuration from Intune and synced the device.

## The app didn't switch to shared mode

**IDmelon Authenticator opens the normal personal screen, not the shared sign-in screen.**

Cause: no `api_key`. Reading that key is the switch; there is no separate toggle.

Fix:

1. Set `api_key` to a **Shared Mobile** key from the IDmelon Admin Panel.
2. Spell it exactly `api_key`, all lowercase.
3. Push it in the Apple namespace `com.apple.configuration.managed`; never author the
   internal `IDmelon` namespace.
4. Re-sync and reopen the app.

See [Configuration keys](../configuration_keys) for the canonical value.

## Activation failed or the device won't register

**The app reaches the shared screen but fails to activate or never finishes registering.**

Cause: wrong `base_api_url` (on-premise only), or a stale device token.

Fix:

1. Cloud: remove `base_api_url` entirely. Its absence is normal.
2. On-premise: set `base_api_url` to your server's exact https URL. See
   [Configuration keys](../configuration_keys).
3. Stale token: re-pair — remove and re-add the device record, then sync.

## Badge or face login won't start

**No badge prompt, no camera prompt, or the wrong prompt appears.**

Cause: `shared_login_method` doesn't match the device; missing or malformed falls back to
badge/hub.

Fix:

1. Pick the method. Badge/hub is the default (tap arrives over the Hub, no iPad accessory);
   face uses the iPad camera, no reader.
2. Paste the exact block. Badge over the Hub:

```xml
<key>shared_login_method</key>
<dict>
  <key>type</key>
  <string>badge</string>
  <key>model</key>
  <string>hub</string>
</dict>
```

3. Face on the iPad camera:

```xml
<key>shared_login_method</key>
<dict>
  <key>type</key>
  <string>face</string>
  <key>model</key>
  <string>hid</string>
</dict>
```

4. Meant face but see a badge prompt? Your face block is malformed — re-check `type` and
   `model`.
5. For badge/hub, confirm the reader feeding the Hub is powered and reachable.

See [Login methods](../login_methods) for every model and block.

## Microsoft apps keep asking for a password

**A user signs in to IDmelon, but Teams or Outlook still prompts for a Microsoft password.**

Cause: Microsoft (MSAL) sign-in is off or misconfigured.

Fix:

1. Set `use_msal` to `true` — the on-switch for Microsoft sign-in.
2. Set `azure_client_id` to your Entra app registration's Application (client) ID — the only
   Microsoft key the iPad needs; skip `azure_tenant_id` (defaults to `common`).
3. Add the redirect URI `msauth.com.idmelon.idmelon-2://auth` to the Entra app registration.
4. Assign both SSO extension profiles — Microsoft Entra and IDmelon — to the device.
5. Install Microsoft Authenticator — the MSAL flow depends on it.

Full walkthrough: [Set up a shared iPad](../set_up_a_shared_ipad). Values:
[Configuration keys](../configuration_keys).

## The Microsoft flow doesn't act shared

**Sign-in works, but the account sticks between users.**

Cause: Microsoft Authenticator is not in shared-device mode. This flag lives on the
**Microsoft Authenticator** app config, not IDmelon.

Fix:

1. In Intune, open the app configuration policy targeting **Microsoft Authenticator** (a
   separate policy from the IDmelon one).
2. Set `sharedDeviceMode` to Boolean **true** — a real Boolean, not the string `"true"`.
3. Re-sync the device.

![Microsoft Authenticator shared device mode](/images/vendor/shared_ipads_new/shared_ipad_shared_device_mode.PNG)

## A shortcut tile opens the wrong thing

**A home-screen tile opens the wrong site, a broken link, or a generic sign-in.**

Cause: the tile's `url` is wrong, or its token is misspelled so it isn't substituted.

Fix:

1. Open the `shortcut_list` entry and check the `url`.
2. Use a supported token, spelled exactly: `{email}`, `{username}`, `{name}`, `{realm}`,
   `{organization}`, `{tenantID}`. Example:

```
https://myapps.microsoft.com/?login_hint={email}
```

3. Re-sync and tap the tile again.

See [Home page Customization](../customization) for the full tile format and token list.

## Unenrolled users get stuck

**A person who has never enrolled taps to sign in and hits a dead end.**

Cause: `self_service_url` is missing or points at a workflow that doesn't exist.

Fix:

1. In the IDmelon Admin Panel, create the self-service enrollment workflow.
2. Copy its URL into `self_service_url`. See [Configuration keys](../configuration_keys).
3. Confirm the URL loads in a browser before pushing it.

## Still stuck — send logs to support

1. On the iPad, open IDmelon Authenticator and use **Share log** to export the log file.
2. Send it to support with the device's values: which managed app configuration the iPad
   has, the `shared_login_method` you set, whether `use_msal` is on, and whether the device
   is cloud or on-premise.
