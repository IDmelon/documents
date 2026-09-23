---
title: "Self-Service Enrollment"
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

Self-service enrollment allows for a user-led onboarding approach. This approach will allow users to add their own security keys without requiring an admin's assistance, and it works best when set up in a kiosk. Admins can find their workspace's self-service link by navigating to the Customization tab and clicking on the **Open in new tab** link.

![Self-Service IDmelon 1](/images/vendor/self_service/self_service_1.png)

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

To upload images, please contact an IDmelon's support team member with the images that you would like to upload. They will review and whitelist them to be used for your self-service page.

Moreover, by default, self-service is only available for card enrollment. If you wish to activate other identifiers, please contact a member from IDmelon's support team.

### Pre-Provisioning Security Keys

Before allowing users to use the self service, create a provisioning request with the type of identifiers that you would like

1. Navigate to **Provisioning** > **New Request**
2. Select the user or user groups to which target self-service for and click on Next
3. Select the type of security key device to provision (including its Storage Type and Integrated RPs like Entra ID) and click on Next
4. Select **Self-Service** as the provisioning strategy
a. Users will receive an email if the **Send activation email** option is checked
5. Review the selected information and click on Create Request
6. Click on **Close**, no need to click on **Provision Now**

Now, these users will be allowed to use self-service.

### Verification Step

After users have presented the credential that they would like to enroll to IDmelon, users must verify that they are who they say they are. For this, you can set up your organization with one of the following methods:

- Email OTP
- SMS OTP
- User attribute input (e.g., full name, employee ID, etc.)
- Identity verification with Nametag ([docs](/docs/for_administrators/authentication/identity_providers/nametag/))
- Entra ID OIDC ([docs](/docs/for_administrators/authentication/identity_providers/entra_id_oidc/))
- Custom OIDC ([docs](/docs/for_administrators/authentication/identity_providers/add_an_oidc/))

## Using Self-Service

Set up the self-service page in a workstation and users will follow the three simple steps set up to enroll their security key.
