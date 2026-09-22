---
title: "Self-Service Enrollment Flow"
description: ""
lead: ""
date: 2026-05-26T00:00:00+00:00
lastmod: 2026-05-26T00:00:00+00:00
draft: false
images: []
menu:
  docs:
    parent: "security_key_enrollment_flows"
weight: 33510
toc: true
mermaid: true
---

Outline:
1. Create provisioning task
2. Self-service link
2.a. (Optional) Customize self-service
3. Select identifiers
4. Go through self-service

Self-service enrollment allows for a user-led onboarding approach. This approach will allow users to add their own security keys without requiring an admin's assistance, and it works best when set up in a kiosk. Admins can find their workspace's self-service link by navigating to the Customization tab and clicking on the **Open in new tab** link 

The self-service enrollment process is divided into three main steps:

{{< mermaid >}}
flowchart LR
    P1["Step 1: Identify the User"]
    P2["Step 2: Verify the User"]
    P3["Step 3: Configure PIN"]

    P1 --> P2 --> P3

    style P1 fill:#e8f7ee,stroke:#91c7a1,stroke-width:2px
    style P2 fill:#fff3dd,stroke:#f0c36d,stroke-width:2px
    style P3 fill:#fde5e5,stroke:#ef8c8c,stroke-width:2px
{{< /mermaid >}}

In most deployments:

- **Step 1** consists of presenting an identifier (e.g., card, smartphone, biometrics) to enroll for a user.
- **Step 2** confirms that the matched user is who they say they are before the security key is provisioned.
- **Step 3 (optional)** is shown only when the applied security key policy requires a PIN.

## Self-Service Setup

By default, self-service is available in all workspaces without a limit on how many security keys a user can enroll. To find your organization's self-service link, navigate to the **Customization** tab and clicking on the **Open in new tab** link. 

If you would like to limit or block the use of self-service, please contact a member from IDmelon's support team.

### Customizing the Self-Service Page

Customize available languages, titles, text boxes, buttons, and images of your self-service page by navigating to **Customization** > **Web Pages** > **Self-Service**. After finishing your customizations, click on the **Save & Close** button in the bottom right corner.

To upload images, please contact a member from IDmelon's support team, and send them the images that you would like to upload.

### Pre-Provisioning Security Keys

To target specific users for self-service, please follow these steps:

1. Navigate to **Provisioning** > **New Request**
2. Select the user or user groups to which target self-service for and click on Next
3. Select the type of security key device to provision (including its Storage Type and Integrated RPs like Entra ID) and click on Next
4. Select **Self-Service** as the provisioning strategy
a. Users will receive an email if the **Send activation email** option is checked
5. Review the selected information and click on Create Request
6. Click on **Provision Now** to create **Pending** security keys for the selected users

Now, these users will be allowed to use self-service.

### Verification Step

After users have presented the credential that they would like to enroll to IDmelon, users must verify that they are who they say they are. For this, you can set up your organization with one of the following methods:

- Email OTP
- SMS OTP
- User attribute input (e.g., full name, employee ID, etc.)
- Identity verification with Nametag
- Entra ID Login

## Self-Service Enrollment Phases

The self-service enrollment process can generally be divided into three phases:

| Phase | Purpose | Available Options / Notes |
| --- | --- | --- |
| **Phase 1 - Identify the User** | Users provide information so the system can uniquely identify them. | User ID / UPN, Employee ID, Phone Number, First Name + Last Name, Additional Attributes, or a combination of these. The selected combination should uniquely identify a user and should be based on information users commonly know. |
| **Phase 2 - Verify the User** | After identification, the system verifies that the matched user is correct. | Email verification code, SMS verification code, Automatic verification using Code/API, Self-confirmation, or None. |
| **Phase 3 - Configure PIN** | Users configure their PIN if required by the security key policy. | PIN setup is controlled by the configured security key policy. If required, users will be prompted to create and confirm their PIN during enrollment. |

## Verification Options

Below is a summary of the verification options in Phase 2:

| Verification Option | Description | Example Use Case |
| --- | --- | --- |
| **Email Code** | Sends a verification code to the user's registered email address. | Useful when email is a trusted and accessible channel for users. |
| **SMS Code** | Sends a verification code to the user's registered phone number. | Useful when mobile phone numbers are available and trusted. |
| **Automatic using Code / API** | Uses an existing system or association table to verify the user automatically. | For example, if a PACS, badge system, HR system, or internal database already maps users to badge numbers, that data can be validated through API without migrating it into IDmelon. |
| **Self-confirmation** | Shows the matched user information and asks the user to confirm it is correct. | Useful to prevent mistakes when users enter Employee ID, phone number, or other identifiers. |
| **None** | Skips the verification step after the user is identified. | Useful when the customer decides identification alone is sufficient for the enrollment flow. |
