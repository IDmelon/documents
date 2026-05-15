---
title: "Import Users from Entra ID"
description: "Learn to import users from Entra ID in to your IDmelon panel."
lead: ""
date: 2023-09-20T13:34:58+03:30
lastmod: 2025-04-15T15:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "add_or_import_users"
weight: 31200
toc: true
---

Organizations with **Azure AD-joined** or **Hybrid AD-joined** environments can seamlessly import users or user groups from Microsoft Entra ID into the IDmelon Admin Panel. This integration allows IDmelon to provision **Microsoft Passkeys** on behalf of users directly within the panel.

## Prerequisites

A global admin role in Entra is required to link the application.

## Manual User Import from Entra ID

1. Go to the IDmelon **Admin Panel > Users > All Users**.
2. Click **Import Users** and select **Entra ID** (Azure AD).
3. Microsoft will ask you to grant certain permissions to integrate with your Entra. To manually add required permissions or scope them to specific user groups, please refer to [this guide](/docs/for_administrators/app_integrations/api_integrations/integrate_azure_application/). After the required permissions are granted, select the group(s) of users you'd like to import.
4. Choose one of the following options:
   - **Import New Users:** Only imports users not already present in the panel.
   - **Update Existing Users and Add New Ones:** Updates existing users (e.g., syncing the panel owner with Entra ID) and adds any new users selected.
   - **Update Existing Users:** Updates user data for those already imported.
5. Click **Confirm** to import the selected users into the IDmelon Admin Panel.

## Automate User Import via Workflow

Admins can also **automate** the import process based on a **defined schedule**.

To create a workflow:

1. Navigate to **Workflows > Entra User ID Import**.
2. Enter a name for the workflow (e.g., Azure AD User Import).
3. On the **Configuration page**:
   - Select your tenant.
   - In the Group field, specify which **group(s)** of users to import. Leave this field empty to import all users from your Entra ID tenant.
   - Set the **frequency** and **time** for automatic user import.
4. Click **Next**, review your settings, and click **Confirm**.

Once configured, the workflow will **automatically** run based on the defined schedule, with no further admin interaction required.
