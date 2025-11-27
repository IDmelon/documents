---
title: "Configuring the Microsoft Enterprise SSO plug-in"
description: "Configuring the Microsoft Enterprise SSO plug-in"
lead: ""
date: 2023-09-20T15:19:33+03:30
lastmod: 2023-09-20T15:19:33+03:30
draft: false
images: []
menu:
  docs:
    parent: "shared_mobile_devices"
weight: 62500
toc: true
---

**Microsoft Enterprise SSO plug‑in** enables seamless single sign‑on across apps and websites using corporate credentials. On shared iPads, it streamlines user login by automatically handling authentication and reducing repeated sign‑ins.

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
    - **Extension ID**: `com.microsoft.azureauthenticator.ssoextension`
    - **Team ID**: Not required for iOS
    - **URLs:**

        ```json
        https://login.microsoftonline.com
        https://login.microsoft.com
        https://sts.windows.net
        https://login.partner.microsoftonline.cn
        https://login.chinacloudapi.cn
        https://login.microsoftonline.us
        https://login-us.microsoftonline.com
        ```

    - **Additional configuration:**
    | key                             | type    | value                                  |
    |---------------------------------|---------|----------------------------------------|
    | AppPrefixAllowList              | String  | com.idemlon.,com.microsoft.,com.apple. |
    | browser_sso_interaction_enabled | Integer | 1                                      |
    | disable_explicit_app_prompt     | Integer | 1                                      |
