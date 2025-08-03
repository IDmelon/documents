---
title: "Provision FIDO2 Passkeys for Okta"
description: ""
lead: ""
date: 2025-07-31T12:00:00+03:30
lastmod: 2025-07-31T12:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "manage_passkeys_and_credentials"
weight: 34110
toc: true
---

Admins can assign Okta passkeys to users via the IDmelon Admin Panel by integrating their Okta environment. Once configured, users can use their security keys to authenticate to Okta-supported applications and services.

## Prerequisites

- You must have an **Okta origin** with access to the **Okta Admin Console**.
- A valid **Okta API Token** must be created via **Security → API → Tokens** in the Okta Admin Console.
- Okta users must be available and synced within the IDmelon Admin Panel (via group or directory import).
- The integration with Okta must be configured in the **App Integration** section of the IDmelon Admin Panel.

## Assign Okta Passkey One by One

Admins can assign Okta passkeys individually through the user profile in the Admin Panel.

1. Log in to the **IDmelon Admin Panel**.
2. Navigate to **Users → All Users**.
3. Select the user you wish to assign a passkey to.
4. Go to the **Passkeys** tab.
5. Click **Add Passkey** and choose **Okta**.
6. Click **Create** in the popup window.

The passkey will be assigned to the user and will become active once the security key is initialized by the user.

## Automatic Okta Assignment via Workflows

Admins can automate the assignment of Okta passkeys using the workflow feature.

1. Navigate to **Workflows → Okta Passkey Provisioning**.
2. Enter a name for the workflow (e.g., "Auto Okta Provisioning") and click **Next**.
3. On the configuration page:
   - The **Okta Origin** field will be automatically populated if the Okta integration has already been set up.
4. Set the **trigger** condition:
   - **New User Imported:** Assigns the passkey when a user is added to the panel.
   - **New Security Key Activated (Recommended):** Assigns the passkey once the user activates their security key.
5. Continue to the next step to **review** and **submit** the workflow.

Once activated, the workflow will automatically assign Okta passkeys to applicable users based on the chosen trigger, requiring no further manual action.
