---
title: "Provision FIDO2 Passkeys for Okta"
description: "Configure Okta FIDO2 authenticator policies and assign passkeys to users"
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

Administrators can assign Okta passkeys to users via the IDmelon Admin Panel by integrating their Okta environment. Once configured, users can use their security keys to authenticate to Okta-supported applications and services.

## Prerequisites

Before configuring Okta passkey provisioning, ensure you have:

- **Okta API Integration**: Complete the [Okta API Integration](/docs/for_administrators/app_integrations/api_integrations/integrate_okta/) setup first.
- **Okta Admin Console Access**: Ensure you have access to the **Okta Admin Console** with administrative privileges.
- **User Synchronization**: Okta users must be available and synchronized in the IDmelon Admin Panel (via group or directory import).
- **User Groups**: You must have an **Okta group** containing users to which the WebAuthn policy will be applied.

## Overview

This guide walks you through configuring Okta FIDO2 authenticator settings and policies to enable passkey authentication for your users. The process involves setting up authenticator options, enrollment policies, and authentication policies in Okta.

## Setting up Okta FIDO2 Authenticator Options

1. Navigate to **Security > Authenticators** in your Okta Admin Console, then select **Setup** options and click **Add authenticator**.

   ![Okta - Add Authenticator](/images/vendor/provisioning/okta/03.png)

2. Select the **FIDO2 (WebAuthn)** authenticator from the list.

   ![Okta - Select FIDO2 Authenticator](/images/vendor/provisioning/okta/04.png)

3. Configure the authenticator settings:
   - Select **Preferred** option for the authenticator
   - Click **Add** to complete the setup

   ![Okta - Configure FIDO2 Authenticator](/images/vendor/provisioning/okta/05.png)

## Adding Okta Enrollment Policy

1. Navigate back to **Security > Authenticators** and select **Enrollment** options.
2. Click **Add a policy**.

   ![Okta - Add Enrollment Policy](/images/vendor/provisioning/okta/06.png)

3. Fill in the required fields as shown in the image:
   - **Policy name**: Enter a descriptive name (e.g., "FIDO2 Enrollment Policy")
   - **Assign to groups**: Select the groups that should use FIDO2 (WebAuthn) authenticator
   - **Authenticators**: Select authenticator options as appropriate
   - **Allowed authenticators**: Choose "Any WebAuthn authenticator"

   ![Okta - Configure Enrollment Policy](/images/vendor/provisioning/okta/07.png)

4. In the newly opened **Add Rule** window, provide a rule name and click **Create Rule**.

   ![Okta - Create Enrollment Rule](/images/vendor/provisioning/okta/08.png)

## Setting up Okta Authentication Policy

1. Navigate to **Security > Authentication Policies** and select **Okta Dashboard** (or any application you want to protect).

   ![Okta - Authentication Policies](/images/vendor/provisioning/okta/09.png)

2. In the **Rule** section, click **Add Rule**.

   ![Okta - Add Authentication Rule](/images/vendor/provisioning/okta/10.png)

3. Configure the authentication rule with the following settings:
   - **Rule name**: Provide a descriptive name
   - **Assign to groups**: Select the same groups that have the authenticator policy enabled
   - **Possession factor**: Select this option
   - **Phishing resistant**: Select this option
   - **FIDO2 (WebAuthn) authenticator**: Select this authenticator

   ![Okta - Configure Authentication Rule](/images/vendor/provisioning/okta/11.png)

   ![Okta - Authentication Rule Settings](/images/vendor/provisioning/okta/12.png)

> **Important**: Users in the specified groups can only access the specified application when all authentication policies are met.

## Assigning Okta Passkeys

### Individual Passkey Assignment

Administrators can assign Okta passkeys individually through the user profile in the Admin Panel:

1. Log in to the **IDmelon Admin Panel**.
2. Navigate to **Users → All Users**.
3. Select the user you wish to assign a passkey to.
4. Go to the **Passkeys** tab.
5. Click **Add Passkey** and choose **Okta**.
6. Click **Create** in the popup window.

The passkey will be assigned to the user and will become active once the security key is initialized by the user.

### Automatic Passkey Assignment via Workflows

Administrators can automate the assignment of Okta passkeys using the workflow feature:

1. Navigate to **Workflows → Okta Passkey Provisioning**.
2. Enter a name for the workflow (e.g., "Auto Okta Provisioning") and click **Next**.
3. On the configuration page:
   - The **Okta Origin** field will be automatically populated if the Okta integration has already been set up.
4. Set the **trigger** condition:
   - **New User Imported**: Assigns the passkey when a user is added to the panel
   - **New Security Key Activated (Recommended)**: Assigns the passkey once the user activates their security key
5. Continue to the next step to **review** and **submit** the workflow.

Once activated, the workflow will automatically assign Okta passkeys to applicable users based on the chosen trigger, requiring no further manual action.
