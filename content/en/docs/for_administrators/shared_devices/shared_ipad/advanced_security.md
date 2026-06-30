---
title: "Advanced security options"
description: "Passkey mode, one-time-use passkeys, and the authentication backend."
lead: "Security toggles for advanced deployments."
date: 2026-06-30T00:00:00-07:00
lastmod: 2026-06-30T00:00:00-07:00
draft: false
images: []
menu:
  docs:
    parent: "shared_ipad"
weight: 62350
toc: true
---

Three optional keys. Most deployments leave them alone. Test on one device first.
For the canonical type and default of each, see [Configuration keys](../configuration_keys).

## Passkeys on the shared iPad

`shared_device_passkeys` turns on passkey (FIDO2) mode. Use it only if your sign-in target
expects passkeys.

```xml
<key>shared_device_passkeys</key>
<true/>
```

> Requires iPadOS 17 or later. No effect on older iPads.

## One-time-use passkeys

`one_time_use_passkeys` spends each passkey after one sign-in. Only takes effect when
`shared_device_passkeys` is also on.

```xml
<key>shared_device_passkeys</key>
<true/>
<key>one_time_use_passkeys</key>
<true/>
```

> Requires iPadOS 17 or later.

## Authentication backend

`shared_authentication_backend` picks the backend protocol. Value is lowercased before
reading (`Legacy` = `legacy`). Default is `legacy`.

- `legacy` — leave it here.
- `authenticator` — newer protocol; must match your tenant provisioning.

Change only if IDmelon support tells you to. The wrong value breaks sign-in for the whole
device.

```xml
<key>shared_authentication_backend</key>
<string>legacy</string>
```
