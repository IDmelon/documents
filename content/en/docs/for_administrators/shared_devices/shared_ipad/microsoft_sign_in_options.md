---
title: "Microsoft sign-in options"
description: "Advanced MSAL settings beyond the standard shared-iPad setup."
lead: "For non-standard Microsoft setups."
date: 2026-06-30T00:00:00-07:00
lastmod: 2026-06-30T00:00:00-07:00
draft: false
images: []
menu:
  docs:
    parent: "shared_ipad"
weight: 62340
toc: true
---

Advanced Microsoft sign-in keys. Most teams never touch them. The standard
[Set up a shared iPad](../set_up_a_shared_ipad) flow covers a normal Entra tenant.

For the type, allowed values, and default of every key, see
[Reference → Configuration keys](../configuration_keys) (the canonical value).

## `use_msal`

Turns on Microsoft Entra (MSAL) login. The setup page sets `use_msal` and `azure_client_id`
(the two keys MSAL needs).

## Lock sign-in to one tenant

`azure_tenant_id` is optional. Absent = `common`. Set it to lock sign-in to one tenant.

```xml
<key>azure_tenant_id</key>
<string>YOUR_TENANT_ID</string>
```

## Browser-based sign-in

Default is embedded in-app sign-in. Move to a browser if the embedded flow breaks
(conditional access, federation).

- `msal_browser_login` — runs sign-in through the system browser flow.
- `msal_browser_login_url` — base URL the browser flow starts from. Change only for a
  non-commercial cloud.
- `external_login` — opens sign-in in Safari instead of the in-app browser.

```xml
<key>msal_browser_login</key>
<true/>
<key>msal_browser_login_url</key>
<string>https://login.microsoftonline.com</string>
```

## Pre-fill the user

- `use_login_hint` — pre-fills the username. On by default. Turn off only if your tenant
  rejects a pre-filled hint.
- `domain_hint` — pre-selects an organization. Set to your domain to skip the account picker.

```xml
<key>domain_hint</key>
<string>contoso.com</string>
```

## Name the Microsoft provider

`microsoft_sp_name` — label for the Microsoft provider on the sign-in screen. Default
`entra_id`.

```xml
<key>microsoft_sp_name</key>
<string>company_login</string>
```

## Self-hosted / on-premise

Skip if you use IDmelon's cloud.

- `base_api_url` — full https URL of your on-premise server.
- `device_id` — pre-provisions one iPad's unique id. Per-device: each iPad needs its own
  value. Never push one `device_id` to a fleet — it collides across devices.

```xml
<key>base_api_url</key>
<string>https://idmelon.example.com</string>
```

## Legacy note

On iPad, `shared_device_mode` is **not** the on/off switch — the presence of `api_key` makes
the app a shared device (see [Set up a shared iPad](../set_up_a_shared_ipad)). Here it only
acts as an MSAL fallback. (It is the real switch on Android.)
