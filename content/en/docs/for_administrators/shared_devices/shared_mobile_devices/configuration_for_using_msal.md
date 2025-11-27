---
title: "Configuration for Using MSAL"
description: ""
lead: ""
date: 2023-09-20T15:19:33+03:30
lastmod: 2023-09-20T15:19:33+03:30
draft: false
images: []
menu:
  docs:
    parent: "shared_mobile_devices"
weight: 62400
toc: true
---

Microsoft Authentication Library (MSAL) enables a unified sign‑in experience across Microsoft applications. On shared iPads, MSAL ensures that once a user signs in, the authentication state is seamlessly applied to apps like Teams, Outlook, Word, Excel, etc, eliminating the need for repeated logins. This provides a consistent and secure single sign‑on environment for all Microsoft apps.

## Integrating with IDmelon SSO

To enable **Single Sign‑On (SSO)** login capabilities, your environment must be integrated with **IDmelon**. The IDmelon SSO extension handles authentication flows for users whose identity provider is set to IDmelon.

To configure, see [IDmelon Configuration as IDP](../../../app_integrations/integration_guides/app_integration_guides/entraid/#idmelon-configuration-as-idp).

## Register an application in Microsoft Entra ID

This process is essential for establishing a trust relationship between IDmelon Authenticator and the Microsoft identity platform.

Follow these steps to create the app registration:

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com/).
2. If you have access to multiple tenants, use the **Settings** icon  in the top menu to switch to the tenant in which you want to register the application.
3. Browse to **Entra ID > App registrations** and select **New registration**.
4. Enter a meaningful Name for the app, for example `IDmelon-Authenticator-MSAL`.
5. Under **Supported account types**, specify who can use the application. We recommend you select **Accounts in this organizational directory only**.
6. For the **Redirect URI**, select **Mobile and desktop applications** as the platform, and then, in the URL box, enter:

    ```text
    msauth.com.idmelon.idmelon-2://auth
    ```

7. Select **Register** to complete the app registration.
8. Under the **Manage** section, select **API permissions**.
9. Make sure the delegated permission **User.Read** is present. If it is not, select **Add a permission → Microsoft Graph → Delegated permissions**, then check **User.Read** and click **Add permissions**.
10. Select **Grant admin consent** for < tenant name >, then select **Yes**.
11. From the **Overview** section, the **Application (client) ID** is available. You can use this value as the configuration for [azure_client_id](../shared_device_mode_configuration_ios_ipad/#azure_client_id-string-optional).

![Client ID](/images/vendor/shared_ipads/shared_ipads_entra_app_registration_client_id.png)

## Microsoft Enterprise SSO plug-in

For MSAL integration, the [**Microsoft Enterprise SSO plug-in**](../configuration_for_microsoft_enterprise_sso) must be configured on the device.

## Configuring the IDmelon Enterprise SSO plug-in

The IDmelon SSO extension is responsible for authenticating shared users in the IDmelon panel.

To configure, follow the steps below:

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Devices > Manage devices > Configuration > Create > New policy**.
3. Enter the following properties:
    - **Platform**: Select `iOS/iPadOS`.
    - **Profile type**: Select `Templates > Device features`.
4. Select **Create**:
5. In Basics, enter the following properties:
    - **Name**: Enter a descriptive name for the policy.
    - **Description**: Enter a description for the policy.
6. Select **Next**.
7. In Configuration settings, select **Single sign-on app extension**, and configure the following properties:
    - **SSO app extension type**: `Redirect`.
    - **Extension ID**: `com.idmelon.idmelon-2.ssoextension`
    - **Team ID**: `4A6ZQ29Y2F`
    - **URLs:**

        ```json
        https://panel.idmelon.com/auth/sign-in
        ```
