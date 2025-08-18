---
title: "Provision FIDO2 Passkeys for Microsoft"
description: "Configure Microsoft FIDO2 passkey provisioning for Azure AD-joined or Hybrid environments"
lead: ""
date: 2025-04-15T15:00:00+03:30
lastmod: 2025-04-15T15:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "manage_passkeys_and_credentials"
weight: 34100
toc: true
---

Administrators can assign Microsoft passkeys to users via the IDmelon Admin Panel in Azure AD-joined or Hybrid environments. This allows users to sign in to Microsoft services, including Windows and office.com, using their security key.

## Prerequisites

Before configuring Microsoft passkey provisioning, ensure you have:

- **Azure Application Integration**: Complete the [Azure Application Integration](/docs/for_administrators/app_integrations/api_integrations/integrate_azure_application/) setup first.
- **FIDO2 Security Key**: Must be enabled in **Azure Portal → Entra ID → Authentication Methods** for the targeted users.
- **User Import**: Users must be imported from **Microsoft Entra ID (Azure AD)** and the necessary permissions must have been granted by the administrator during the initial integration process.

## Overview

This guide explains how to assign Microsoft passkeys to users in Azure AD-joined or Hybrid environments. You can assign passkeys individually or automatically through workflows.

## Assigning Microsoft Passkeys

### Individual Passkey Assignment

Administrators can assign Microsoft passkeys to users that have been imported into the IDmelon Admin Panel via Entra ID:

1. Log in to the **IDmelon Admin Panel**.
2. Navigate to **Users → All Users**.
3. Select the target user.
4. Click on the **Passkeys** tab.
5. Click **Add Passkey** and choose **Microsoft**.
6. In the prompt window, click **Create**.

The Microsoft passkey is now assigned to the user. Once the user activates their security key, they can use it to sign in to office.com and Windows login.

### Automatic Passkey Assignment via Workflows

Administrators can use workflows to assign Microsoft passkeys to multiple users automatically:

1. Go to **Workflows → Microsoft Passkey Provisioning**.
2. Name the workflow (e.g., "Automatic MS Passkey Provisioning") and click **Next**.
3. On the configuration page, the **tenant** will be pre-filled because the administrator has already used the tenant to import users from **Entra ID**.
4. Leave the **Group** field empty to apply the workflow to all users or select a specific group.
5. Set the **trigger**:
   - **New User Imported**: Assigns the passkey when a user is imported
   - **New Security Key Activated (Recommended)**: Assigns the passkey when the user activates their security key
6. After setting the trigger, proceed to the next page to **review** and **submit** the workflow.

Once submitted, the workflow will automatically assign Microsoft passkeys to users based on the selected trigger — no further administrator interaction is required.
