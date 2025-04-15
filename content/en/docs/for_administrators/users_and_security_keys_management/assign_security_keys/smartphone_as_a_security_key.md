---
title: "Smartphone as a Security Key"
description: ""
lead: ""
date: 2025-04-15T15:00:00+03:30
lastmod: 2025-04-15T15:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "assign_security_keys"
weight: 33200
toc: true
---

## Prerequisites

Before assigning a smartphone as a security key, users must:

1. Install the IDmelon Authenticator App available on both the App Store (iOS) and Google Play Store (Android).
2. Enable IDmelon as the Default Passkey Autofill Provider:
3. Open the IDmelon Authenticator App
   - Tap the menu (☰) icon
   - Go to Autofill Passkey
   - Enable IDmelon in the phone’s system settings

## Add Smartphone as a Security Key (One by One)

Admins can manually assign a **smartphone** as a **security key** to a user.

1. Go to the IDmelon Admin Panel > Users > All Users.
2. Select the user you want to assign the key to and open their User Details.
3. Navigate to the **Security Keys** tab and click **+ New Security Key**.
4. Choose Smartphone as the key type, then click Next.
5. Choose one of the activation options:
   - **Activate Now:** If the user is present and ready to activate immediately.
   - **Activate via Email:** Sends an activation link to the user’s registered email.
6. Once the user receives the email, they can open the IDmelon Authenticator App, select **Scan QR Code**, or use the **Work or School** option to register manually.

Once activated, the smartphone can be used as a security key for web authentication, including Microsoft 365 logins.

> Note: If the user also needs to log in to **Windows** with their **smartphone**, one of the following setups is required:

- **Dedicated PC:** Install the IDmelon Pairing Tool from the IDmelon website and pair it with the user’s phone.
- **Shared PC:** Use the IDmelon Reader connected to the machine to enable smartphone-based login.

## Automate Smartphone Assignment via Workflow

For large-scale deployments, administrators can **automate** the assignment of smartphone as a security keys through a **workflow** in the IDmelon Admin Panel. This ensures consistent and scalable deployment across all users or selected groups.

1. Navigate to **Workflows > Security Key Device Assignment**.
2. Enter a name for your workflow (e.g., Smartphone Security Key Deployment).
3. On the **Configuration** page:
   - Select the **group(s)** you want to apply this configuration to.
   - To apply it to all users in the panel, leave the group field empty.
   - Choose **Smartphone** as the type of security key.
   - Under **security key setting**, leave the default settings selected (recommended).
4. Click **Next**, review the configuration, and then **Submit** the workflow.

Once submitted, the workflow will automatically run and send activation emails to users who do not yet have an active smartphone as a security key. Users can then activate their key using the IDmelon Authenticator App by scanning the **QR code** or using the **Work or School** option within the app.
