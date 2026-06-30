---
title: "All configuration keys"
description: "Every managed-app-configuration key for the shared iPad: type, values, and default."
lead: "The complete settings reference."
date: 2026-06-30T00:00:00-07:00
lastmod: 2026-06-30T00:00:00-07:00
draft: false
images: []
menu:
  docs:
    parent: "shared_ipad"
weight: 62310
toc: true
---

Set every key in the Apple managed namespace (`com.apple.configuration.managed`) — the XML you
paste in Intune.

## Required

**`api_key`** — turns the app into a shared device; its presence is the switch.  
String · Default: none · a **Shared Mobile** secret from the IDmelon Admin Panel

**`base_api_url`** — points the app at your on-premise IDmelon instead of the cloud.  
String · Default: absent (uses the cloud) · an `https` URL

> No `shared_device_mode` switch on iPad. `api_key` is what makes it a shared device.

## Recommended

**`device_id`** — pre-sets a unique device id for this one iPad.  
String · Default: none · never push one value to a whole fleet

**`shared_login_method`** — how users sign in: badge over the IDmelon Hub, or face on the
iPad camera.  
String **or** Dict · Default: `badge` / `hub` · see [Login methods](../login_methods)

**`authentication_type`** — when a PIN or second factor is required (read lowercased).  
String · Default: `oninit` · one of `none`, `oninit`, `onuse`

**`auto_logout`** — session timeout before the iPad signs the user out.  
String · Default: `never` · `never`, `one-time use`, `5m`, `60m`, `2h`, `4h`, `6h`, `8h`

`badge` / `hub` is the paired-reader path; `face` / `hid` uses the iPad camera (no reader). For
keystroke/smart-card readers or a cloud face server, pass a full block — see
[Login methods](../login_methods).

## Microsoft

Set this group and Microsoft apps (Teams, Outlook) open without a second login. Skip it if you
don't use Microsoft apps. See [Set up a shared iPad](../set_up_a_shared_ipad).

**`use_msal`** — turns on Microsoft Entra (MSAL) login; the on-switch.  
Boolean · Default: `false` · set `true` to enable Microsoft sign-in

**`azure_client_id`** — your Entra app (client) id; the only required Microsoft key.  
String · Default: none · the application GUID

**`azure_tenant_id`** — OPTIONAL. Your Entra tenant.  
String · Default: `common` · set it only to lock sign-in to a single tenant

**`microsoft_sp_name`** — label shown for the Microsoft service provider.  
String · Default: `entra_id` · any name

**`msal_browser_login`** — uses the system browser flow for sign-in.  
Boolean · Default: `false`

**`msal_browser_login_url`** — base URL for the browser-login flow.  
String · Default: `https://login.microsoftonline.com`

**`domain_hint`** — pre-selects an organization at the Microsoft prompt.  
String · Default: none · a domain

**`use_login_hint`** — pre-fills the username at the Microsoft prompt.  
Boolean · Default: `true`

**`external_login`** — opens Microsoft sign-in in an external browser.  
Boolean · Default: `false`

Redirect URI is `msauth.com.idmelon.idmelon-2://auth`, hardcoded. See
[Microsoft sign-in options](../microsoft_sign_in_options).

## Experience

Home screen and what opens around sign-in. All optional.

**`shortcut_list`** — home-screen tiles users tap after signing in.  
Array of `{title, url, iconName}` · Default: none · see [Home page Customization](../customization)

**`default_sp_for_auto_login`** — auto-starts sign-in for one named provider.  
String · Default: none · an SP name

**`open_url_after_login`** — opens this URL right after login; accepts tokens.  
String · Default: none · a URL

**`open_url_after_logout`** — opens this URL right after logout; accepts tokens.  
String · Default: none · a URL

**`self_service_url`** — where users who aren't enrolled yet are sent.  
String · Default: none · build the workflow in the Admin Panel first, then paste its URL

**`use_internal_browser`** — use the in-app browser instead of Safari.  
Boolean · Default: `true`

Each tile's `iconName` is fetched remotely as `<name>.png`, not bundled. See
[Home page Customization](../customization).

## Advanced security

**`shared_device_passkeys`** — turns on passkey (FIDO2) mode.  
Boolean · Default: `false` · needs iPadOS 17 or later

**`one_time_use_passkeys`** — makes each passkey single-use.  
Boolean · Default: `false` · needs iPadOS 17 or later

**`shared_authentication_backend`** — backend protocol for the shared auth flow (read
lowercased).  
String · Default: `legacy` · `legacy` or `authenticator`

**`shared_device_mode`** — LEGACY on iPad; an MSAL fallback only, not the on/off switch.  
Boolean · Default: `false` · don't set it on iPad

See [Advanced security](../advanced_security) before turning on passkeys.

## Placeholder tokens

`shortcut_list`, `open_url_after_login`, and `open_url_after_logout` accept these tokens in
their URLs. The app swaps each for the signed-in user's value.

| Token | Replaced with |
| --- | --- |
| `{email}` | The user's email |
| `{username}` | The user's username |
| `{name}` | The user's display name |
| `{realm}` | The user's realm |
| `{organization}` | The user's organization |
| `{tenantID}` | The tenant id |

Example:

```xml
<key>open_url_after_login</key>
<string>https://myapps.microsoft.com/?login_hint={email}</string>
```

## Notes that bite you

- **Apple namespace only.** Keys in `com.apple.configuration.managed` have no fallback.
- **`api_key` is the switch**, not `shared_device_mode`, on iPad.
- **`base_api_url` is on-premise only.** Leave it out for the cloud.
- **`device_id` is per-device.** Never push one value to a whole fleet.
- **Passkey keys need iPadOS 17 or later.** They do nothing on older iPads.
- **`auth_url` is Android-only.** The iPad ignores it.
- **Lowercased values.** `authentication_type` and `shared_authentication_backend` ignore case.
- **`auto_logout`:** `never` and `one-time use` mean no timer; any unrecognized value falls back
  to `never`, so a typo leaves the session open.
