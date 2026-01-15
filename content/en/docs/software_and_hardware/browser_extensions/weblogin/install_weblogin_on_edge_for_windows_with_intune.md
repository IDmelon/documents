---
title: "Install on Edge for Windows using Intune"
description: ""
lead: ""
date: 2025-12-30T14:40:56+01:00
lastmod: 2025-12-30T14:40:56+01:00
draft: false
images: [ ]
menu:
    docs:
        parent: "weblogin"
type: docs
weight: 323000
toc: true
---

This guide provides a step-by-step walkthrough for force-installing the WebLogin extension on Microsoft Edge using
Microsoft Intune.

## Prerequisites

- Intune Administrator permissions.

- Windows 10/11 devices enrolled in Intune.

- Microsoft Edge must already be installed on the target devices.

## Installation Details

- **Extension Name:** WebLogin

- **Extension ID:** eagmgpbjpedchliifpgfgogdknnmkaej

- **Update URL:** https://clients2.google.com/service/update2/crx

## Step 1: Create the Configuration Profile

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com/).

2. Navigate to **Devices** > **Windows** > **Configuration**.

3. Click **Create** > **New Policy**.

4. Select the following:

    - **Platform:** Windows 10 and later

    - **Profile type:** Settings catalog

5. Click **Create**.

   ![alt](/images/vendor/weblogin/weblogin_intune_profile.jpg)

6. On the **Basics** tab, enter a name (e.g., Edge - Force Install WebLogin Extension) and an optional description.
   Click **Next**.

## Step 2: Configure the Extension Policy

1. On the **Configuration settings** tab, click **+ Add settings**.

2. In the **Settings picker**, search for "Extension" and select the **Microsoft Edge** > **Extensions**
   category.

3. Check the box for **Control which extensions are installed silently**.

   ![alt](/images/vendor/weblogin/settings_picker_edge.jpg)

4. Close the **Settings picker**.

5. In the configuration pane, toggle the setting to **Enabled**.

   ![alt](/images/vendor/weblogin/config_toggle_edge.jpg)

6. Under **Extension/App IDs and update URLs to be silently installed**, enter the following string exactly:

   ```bash
   eagmgpbjpedchliifpgfgogdknnmkaej;https://clients2.google.com/service/update2/crx
   ```

   ![alt](/images/vendor/weblogin/extension_id_edge.jpg)

7. Click **Next**.

## Step 3: Assignments and Review

1. **Scope tags:** Add if your organization uses them, otherwise click **Next**.

2. **Assignments:** Click **Add groups** and select the device or user group(s) you want to receive the extension.

3. **Review + create:** Verify the settings and the Extension ID string. Click **Create**.

## Step 4: Verification

Once the policy syncs with the target devices (this can be manually triggered by going to **Settings > Accounts > Access
work or school > Info > Sync** on the device):

1. Open **Microsoft Edge**.

2. Go to `edge://extensions`.

3. You should see **WebLogin** in the list. It will have a briefcase icon next to it, indicating it is managed by your
   organization.

4. You can also verify the policy status at `edge://policy`. Look for the `ExtensionInstallForcelist` policy.
