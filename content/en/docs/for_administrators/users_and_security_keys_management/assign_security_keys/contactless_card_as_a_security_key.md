---
title: "Contactless Card (Badge) as a Security Key"
description: ""
lead: ""
date: 2025-04-15T15:00:00+03:30
lastmod: 2025-04-15T15:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "assign_security_keys"
weight: 33100
toc: true
---

Contactless cards (badges) can be used as FIDO2 security keys for secure authentication.

## Prerequisites

Before assigning a contactless card as a security key:

- A **card reader** must be connected to the user's PC.
- The compatible **IDmelon Reader Driver** must be installed. It is available for download on the [IDmelon website](https://idmelon.com/docs/downloads).
- The badge must be supported by card reader.

## Assign Contactless Card One by One

Admins can manually assign a badge to a user through the IDmelon Admin Panel.

1. Navigate to **Users > All Users** and open the desired user's profile.
2. Go to the **Security Keys** tab and click **+ New Security Key**.
   - Select **Card** as the security key type.
   - Review the default settings. Modify them if needed based on your organization’s policy.
3. Click **Next**, then choose one of the activation options:
   - **Activate Now:** If the user is present, they can tap their badge on the connected reader. A PIN will be generated and emailed to the user.
   - **Activate via Email:** Sends an activation link to the user's email.

The user opens the activation email, clicks **Tap to Activate Your Security Key**, taps their badge, enters a **personal PIN**, and completes the activation process.

### Resetting or Changing the Card PIN

Both admins and users can reset or change the PIN associated with the contactless card at any time:

- **From the user's device:** Go to Windows Settings > Sign-in Options > Security Key > Manage to update the PIN.
- **From the IDmelon Admin Panel:** Navigate to Users > User Profile > Security Keys, click the three dots next to the assigned card, select **Reset PIN**, and enter the new PIN.

## Assigning Contactless Card via Self-Service Flow

For organizations that prefer a user-driven onboarding process, IDmelon offers a Self-Service Enrollment URL tailored to each company. This method allows users to independently onboard and activate their contactless card (badge) as a security key on designated workstations.

### Overview

Each company receives a custom self-service URL that can be used to guide users through the security key activation process on specific devices. This is especially useful when setting up shared workstations for onboarding or when handling large-scale deployments.

### How to Request the Self-Service URL

1. In the **IDmelon Admin Panel**, go to: **Security Keys > Workflows > Self-Service Action**
2. Submit a request to IDmelon for generating a self-service onboarding URL for your company.

The custom URL will be generated and made available in the **same section** of the Admin Panel within 24 hours.

### Customization Options

The self-service URL can be tailored to your company’s preferred verification method, including:

- First name and last name
- Employee ID
- Or other internal identifiers as required

These options help ensure that only authorized users can activate their card as a security key.

### User Experience

Users are directed to a designated onboarding workstation configured with a card reader and compatible IDmelon reader driver.

- They access the custom **self-service URL** from the workstation.
- Users tap their badge on the reader.
- They are prompted to verify their identity based on the company-defined method.
- After verification, they set their **own PIN** and **activate** the card as a security key.

Once complete, the badge can be used for secure authentication in supported systems.

## Best Practices for Automatic Enrollment (Contactless Cards)

To streamline onboarding and minimize manual work, IDmelon offers multiple automated methods for assigning contactless cards as FIDO2 security keys. These workflows help organizations scale efficiently while maintaining secure authentication processes.

### Method 1: User Activation via Email

Admins can configure a **workflow** to automatically send activation emails to users who do not yet have an assigned card.

1. Go to **Workflows > Security key Device Assignment**.
2. Enter a name for your workflow (e.g., Card Activation via Email).
3. On the **Configuration** page:
   - Select the target group(s) or leave the group field empty to apply to all users.
   - Choose **Card** as the security key type.
   - Under “Choose how to assign Cards as Security Key,” select **by end-user activation (via email)**.
   - Review the Security Key **Settings** and adjust them based on your organization’s preferences.
4. Click **Next**, review the summary, and click **Submit**.

The workflow will automatically send activation emails to eligible users, allowing them to onboard their card by tapping it and setting their PIN through the activation link.

### Method 2: Automatic Assignment via CSV

If your organization maintains a CSV file containing users' card identifiers (UIDs), you can use it to assign cards in bulk.

1. Go to **Security Keys > Workflows > Bulk card Import via CSV**.
2. Upload the CSV file containing:
   - User UPN.
   - User identifiers (e.g., email, employee ID)
   - Card UIDs
   - PIN

IDmelon will process the file and map each UID to the corresponding user.

Once mapped, the cards will be assigned automatically.

> Note: Ensure the CSV formatting aligns with IDmelon’s import template (by navigating to all users>import **users > CSV file > download CSV template**) for a smooth mapping process.

### Method 3: Automatic Assignment via SCIM Integration (Recommended)

For the best user experience and fully automated enrollment, IDmelon supports assigning cards using **SCIM attributes**—if your Microsoft Entra ID (Azure AD) is already integrated with your internal Access Control System **(ACS)** and passes card identifiers as part of user attributes.

1. Go to **Workflows > Security key Device Assignment**.
2. Name the workflow (e.g., automatic Card Assignment).
3. On the **Configuration** page:
   - Choose the target group(s) or leave the field empty to apply the workflow to all users.
   - Select **Card** as the security key type.
   - Under “Choose how to assign Cards as Security Key,” choose **Automatic Assignment**.
   - Adjust the Security Key Settings to align with company policies.
4. Click **Next**, review, and then **Submit**.

Once the workflow is active, IDmelon will automatically assign cards to users as they are imported—matching card UIDs from mapped SCIM attributes. You can even configure PINs to be generated based on other SCIM attributes, such as date of birth or employee number.

This method ensures hands-off provisioning and a seamless experience for both administrators and end users.
