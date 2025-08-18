---
title: "Okta Integration"
description: "Integrating IDmelon with Okta API for automated passkey provisioning"
lead: ""
date: 2025-08-18T23:05:00+03:30
lastmod: 2025-08-18T23:05:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "api_integrations"
weight: 15
toc: true
---

IDmelon makes it easy to integrate with your Okta organization for automated passkey provisioning. This document describes the integration process, details the necessary configuration steps, and provides testing instructions.

## Prerequisites

Before configuring the Okta API integration, ensure you have:

- Access to the **Okta Admin Console** with administrative privileges
- An **Okta origin** (your Okta workspace domain)
- Users and groups already synchronized in the IDmelon Admin Panel (via group or directory import)

## Overview

The Okta API integration enables IDmelon to automatically provision and manage FIDO2 passkeys for your Okta users. This integration supports both individual passkey assignment and automated provisioning through workflows.

## Generating an Okta API Token

To integrate IDmelon with Okta, you need to create an API token with appropriate permissions:

1. Navigate to **Security > API** in your Okta Admin Console, then select **Tokens** and click **Create Token**.

   ![Okta - Create API Token](/images/vendor/provisioning/okta/01.png)

2. Configure the token settings:
   - **Name**: Enter a descriptive name (e.g., "IDmelon Integration")
   - **IP Restrictions**: Select `Any IP` or configure specific IP restrictions as needed
   - Click **Create Token**

   ![Okta - Token Configuration](/images/vendor/provisioning/okta/02.png)

> **Important**: 
> - The generated API token will be needed in the IDmelon app integration configuration.
> - The API token inherits all the privileges of the user who created it, so ensure the creating user has appropriate administrative permissions.
> - Store the token securely as it will only be displayed once.

## Integrating IDmelon with Okta

Once you have your Okta API token, follow these steps to configure the integration in IDmelon:

1. Log in to the **IDmelon Admin Panel** and navigate to **App Integrations > API**.
2. Select the **Okta API** option.
3. Fill in the required fields:
   - **Origin**: Enter your Okta workspace domain (e.g., `https://yourcompany.okta.com`)
   - **Okta API Key**: Paste the API token you generated in the previous step
4. Click the **Test Connection** icon next to the Connection Status to verify the integration.
5. Click **Save** to complete the configuration.

![IDmelon Panel - Okta API Configuration](/images/vendor/provisioning/okta/p01.png)


> **Note**: For complete passkey provisioning setup, you'll also need to configure Okta FIDO2 authenticator settings. See our [Provision FIDO2 Passkeys for Okta](/docs/for_administrators/users_and_security_keys_management/manage_passkeys_and_credentials/provision_fido2_passkeys_for_okta/) guide for detailed instructions.