---
title: "API Key Management"
description: "Create and manage API keys for the Admin Panel, shared mobile devices, and client operations"
lead: ""
date: 2025-01-02T11:47:00+03:30
lastmod: 2026-03-03T00:00:00+00:00
draft: false
images: []
menu:
  docs:
    parent: "authentication"
weight: 72111
toc: true
---

Administrators use **API Key Management** to create and manage API keys for their workspace. Each key has a name, type, and expiration date. You can create new keys, view existing ones, and—for **Admin**-type keys—reset (regenerate) a key when needed.

## Where to find API Key Management

In the IDmelon Admin Panel, go to **App Integrations > Authentication > API Key Management** to open the API Key Management page. From there you can create, view, and manage API keys.

## Creating an API key

1. On the API Key Management page, click **+ New API Key**.
2. Enter a **Name** for the key (required).
3. Choose the **Type** of key (required). See [API key types](#api-key-types) below.
4. Complete the dialog (e.g. set expiration if applicable) and create the key.
5. Copy and store the key value securely when it is shown; it may not be displayed again.

## API key types

When creating a key, you select one of the following types:

| Type | Use |
|------|-----|
| **Admin** | Full administrative access. Use this type to call the [Admin Panel REST API](/docs/for_administrators/app_integrations/api_integrations/rest_api/) (statistics, users, security keys, activity logs, workspace settings, etc.). |
| **Shared Mobile** | Used for configuring shared mobile devices through iPad for authentication. |
| **Clients** | Used by clients to call authenticator operations endpoints. |

Choose the type that matches how the key will be used (admin automation, shared mobile setup, or client operations).

## Resetting Admin API keys

If the key type is **Admin**, you can **reset** (regenerate) the key from the API Key Management page. Resetting invalidates the current key and issues a new secret; update any scripts or integrations that use the old key.

- **In the panel:** Open **App Integrations > Authentication > API Key Management**, find the Admin key, and use the reset option for that key.
- **Via API:** You can manage workspace API keys programmatically; see the [REST API](/docs/for_administrators/app_integrations/api_integrations/rest_api/) documentation (e.g. workspace API keys endpoint) for automation.

Only **Admin**-type keys support reset. Shared Mobile and Clients keys do not use this reset flow; create a new key if needed.

## Best practices

- **Name keys clearly** (e.g. "Reporting script", "iPad kiosk config") so you can identify their purpose.
- **Set an expiration date** where possible to limit exposure.
- **Store key values securely** and do not share them or commit them to source control.
- **Rotate Admin keys** periodically or after a suspected compromise; use the reset option for Admin keys when rotating.
