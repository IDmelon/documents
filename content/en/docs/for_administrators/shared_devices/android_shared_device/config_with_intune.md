---
title: "Configure Shared Mode with Intune"
description: ""
lead: ""
date: 2024-07-20T14:40:56+01:00
lastmod: 2024-07-20T14:40:56+01:00
draft: false
images: [ ]
menu:
    docs:
        parent: "android_shared_device"
type: docs
weight: 321100
toc: true
---

This document provides instructions for IT administrators to deploy and configure the **IDmelon Authenticator** app on
Android Enterprise devices using **Microsoft Intune**, specifically optimized for **Shared Device Mode**.

## Prerequisites

- A Microsoft Intune license and administrative access to
  the [Microsoft Intune admin center](https://intune.microsoft.com/).
- Android Enterprise enrolled devices (Dedicated, Fully Managed, or Corporate-owned with Work Profile).
- The IDmelon Authenticator app added to your Managed Google Play Store.
- An active administrator account for the [IDmelon Panel](https://panel.idmelon.com).

## Generating the API Key

Before configuring Intune, you must generate a unique API key from the IDmelon Panel to link the mobile app to your
organization.

![alt](/images/vendor/shared_android/get_api_key.jpg)

1. Log in to the IDmelon Panel: [https://panel.idmelon.com](https://panel.idmelon.com).

2. Navigate to **Authentication** > **API Key Management** in the left-hand sidebar.

3. Click the **+ New API Key** button.

4. Configure the key settings:

    - **Name:** Enter a recognizable name (e.g., "Shared Android Devices").

    - **Type:** Select Shared Mobile.

    - **Expiration:** Set an appropriate expiration date according to your security policy.

5. Click Save/Generate and copy the API Key string. You will need this for the Intune configuration in the next step.

> Keep this key secure. It allows the app to authenticate with your workspace.

## Configuration Details

To enable shared functionality and link the app to your workspace, you must apply a **Managed Configuration** using the
following parameters:

| Key                | Type    | Value                                                              | Description                                           |
|--------------------|---------|--------------------------------------------------------------------|-------------------------------------------------------|
| shared_device_mode | Boolean | true                                                               | Enables multi-user optimization and session clearing. |
| api_key            | String  | eyJhbGciOiJIUz...                                                  | Your unique API key for workspace authentication.     |
| auth_url           | String  | https://panel.idmelon.com/auth/sign-in?workspace=YOUR_WORKSPACE_ID | The specific sign-in endpoint for your workspace.     |

## Step-by-Step Setup Instructions

1. Add IDmelon Authenticator to Intune

   ![alt](/images/vendor/shared_android/create_app.jpg)

    1. Navigate to Apps > Android > Create.

    2. Select Managed Google Play app as the App type.

    3. Search for IDmelon Authenticator (com.vancosys.authenticator.business).

    4. Approve and sync the app to your Intune environment.

    5. Click on Refresh, Select the app and assign it as Required for desired users/groups.

2. Create the App Configuration Policy

   ![alt](/images/vendor/shared_android/create_config.jpg)

    1. Go to Apps > Android > Configuration > Create > Managed devices.

    2. Basics:

       ![alt](/images/vendor/shared_android/create_config_basics.jpg)

        - Name: IDmelon Android App Configuration.

        - Platform: Android Enterprise.

        - Profile Type: All Profile Types (or specific to your deployment).

        - Targeted App: Select IDmelon Authenticator.

    3. Settings:

        - Set Configuration settings format to Use configuration designer.

        - Click Add to pick the variables. Select `api_key`, `shared_device_mode`, and `auth_url`.

        - Enter the values provided in the table above.

3. Deployment (Assignments)

    1. Under the Assignments tab, include the Device Groups that represent your shared hardware.

    2. Review and Create.

