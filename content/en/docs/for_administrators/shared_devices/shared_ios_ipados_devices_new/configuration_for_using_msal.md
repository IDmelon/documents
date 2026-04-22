---
title: "Microsoft Entra app for MSAL"
description: ""
lead: ""
date: 2026-04-21T00:00:00-07:00
lastmod: 2026-04-21T00:00:00-07:00
draft: false
images: []
menu:
  docs:
    parent: "shared_ios_ipados_devices_new"
weight: 62400
toc: true
---

This page covers one task only: create the **Microsoft Entra app registration** that provides the
`azure_client_id` value used by the shared iPad Intune policy.

## Prerequisites

- Access to the [Microsoft Entra admin center](https://entra.microsoft.com)
- Permission to create or manage app registrations in the correct tenant
- The shared iPad deployment described in [Set up Shared iPad with MSAL](../shared_device_mode_configuration_ios_ipad)

## Entra app

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com/).
2. If you manage more than one tenant, switch to the tenant used by your shared iPad deployment.
3. Go to **Entra ID > App registrations**.
4. Click **New registration**.
5. Enter a recognizable name such as `IDmelon-Authenticator-Shared-iPad`.
6. Under **Supported account types**, select the option that matches your tenant policy.
   In most deployments, this is **Accounts in this organizational directory only**.
7. Under **Redirect URI**, select **Mobile and desktop applications**.
8. Enter the following redirect URI:

   ```text
   msauth.com.idmelon.idmelon-2://auth
   ```

9. Click **Register**.
10. Open the new app registration and go to **API permissions**.
11. Confirm that delegated permission **User.Read** is present.
12. If it is missing, click **Add a permission > Microsoft Graph > Delegated permissions**,
    select **User.Read**, and add the permission.
13. Click **Grant admin consent** for the tenant if your policy requires it.
14. From the **Overview** page, copy the **Application (client) ID**.

![Entra app registration client ID](/images/vendor/shared_ipads_new/shared_ipads_entra_app_registration_client_id.png)

## Client ID

Once the Microsoft Entra application exists, place its **Application (client) ID** into the
managed app configuration:

```xml
<dict>
<key>use_msal</key>
<true/>
<key>azure_client_id</key>
<string>YOUR_ENTRA_APP_CLIENT_ID</string>
</dict>
```

Do not paste the example value from this document. Use the client ID from your own Entra app
registration.

## Next step

After you have the client ID:

1. Place it in the shared iPad app configuration as `azure_client_id`.
2. Configure **Microsoft Authenticator** with `sharedDeviceMode` set to Boolean `true`.
3. Configure the Microsoft Enterprise SSO profile.

Use this reference page:

- [Microsoft Enterprise SSO profile](../configuration_for_microsoft_enterprise_sso)
