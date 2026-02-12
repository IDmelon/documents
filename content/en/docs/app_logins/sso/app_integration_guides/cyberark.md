---
title: "CyberArk"
description: "How to integrate IDmelon with CyberArk for single sign-on."
lead: ""
date: 2023-09-13T00:57:24+03:30
lastmod: 2023-09-13T00:57:24+03:30
draft: false
images: []
menu:
  docs:
    parent:   
weight: 72200
toc: true
---

> **⚠️ Important Notice**
>
> The content in this document is currently under review and will be updated soon. We are actively working on providing you with the most accurate and up-to-date information. Thank you for your patience and understanding.

In this document you are going to set up `IDmelon` as an external IdP to the `CyberArk`.

## Login to CyberArk administration panel

---

In order to set up the connection, you will need to log into your `CyberArk administration panel`.

## Add Identity Provider

---

In the `CyberArk administration panel`, from the side menu, navigate to the `Settings` menu, then click `Users`.

![alt](/images/vendor/sso/cyberark_dashboard_01.png)

Under the `Sources`, click `External Identity Providers`.

![alt](/images/vendor/sso/cyberark_dashboard_02.png)

Click `Add` to add a new IdP config.

![alt](/images/vendor/sso/cyberark_dashboard_03.png)

In the `External Identity Providers Name`, choose your custom IdP name.

- **External Identity Providers Name**: `Your custom name`

Activate the `Active` button to redirect your users to IDmelon.

- **Active**: `Click to Active`

Choose SAML 2.0 as `Federation Type`.

- **Federation Type**: `SAML 2.0`

For `Federation Domains`, add your custom users' domain as many as your users have.

- **Federation Domains**: `Your custom user domains`

![alt](/images/vendor/sso/cyberark_dashboard_04.png)

**`Then click Inbound Metadata option`**.

There are several options which IDmelon supports, but IDmelon recommends to `Upload IdP configuration from URL`.

> Get all values for `{..}` from your IDmelon panel.
> If you are currently logged in here, you will see the replaced values instead.

- **Upload IdP configuration from URL**: idp_issuer_uri

![alt](/images/vendor/sso/cyberark_dashboard_05.png)

**`Then click **Outbound Metadata option`**.

## IDmelon SAML configuration

---

You should download **`metadata`** file and **`copy values`** of below fields from this **CyberArk panel** to **IDmelon
Panel**.

- **Download Service Provider Metadata**: Download Metadata

![alt](/images/vendor/sso/cyberark_dashboard_06.png)

- Click on **Manual Configuration**.

  - **Assertion consumer service :** Copy **Service Provider Authentication Response URL**.

  - **Entity id :** CN=CyberArk:Customer:{**your custom subdomain in capital words**}

My company Domain is https://aas4313.id.cyberark.cloud/ so my company subdomain which CyberArk provided will be :
**abl4313** so my entity id will be Example : `CN=CyberArk:Customer:ABlL4313` .

- You should copy values of below fields from this CyberArk panel to IDmelon Panel.

![alt](/images/vendor/sso/cyberark_dashboard_11.png)

**`Then click **Authentication option**`**.

In order to map IDmelon and CyberArk users, choose Optional in the `Map federated user to existing directory use`.

- **Map federated user to existing directory use**: `Optional`

![alt](/images/vendor/sso/cyberark_dashboard_07.png)

- Click **Save**.

### Tips

- **Optional**: Selecting `Optional` means authentication of a mapped federation user results in the user of the mapped directory service. If a user cannot be mapped, a new federated user is created.
- **Required**: Selecting `Required` means the user of a federation will authenticate as the matching user of another directory service. If no match is found, login is denied. If `Create cloud user if unable to map` is also enabled, a matched CyberArk Cloud Directory user is created and login is permitted.

## Suffixes for users domain Tips

---

In the `CyberArk administration panel`, from the side menu, navigate to the `Settings` menu, then click `Customization`.

![alt](/images/vendor/sso/cyberark_dashboard_08.png)

Then under the `Login`, choose `Suffix`, Click on `Add` button which is on the Right hand of the dashboard.

![alt](/images/vendor/sso/cyberark_dashboard_09.png)

Add Custom `Login Suffix`, example: username@`idmelon.com`

- **Login Suffix**: `username@Your custom domain`

On the `Advanced` option, users can be mapped and log in with another suffix.

![alt](/images/vendor/sso/cyberark_dashboard_10.png)

- **map users for sso**: If you want to use another suffix domain which does not exist in CyberArk but the user information exists in IDmelon, you have to **uncheck** **`Keep Login Suffix and Mapped Suffix the same`** and choose  **`your custom user domain`**, Example : **username@idmelon.me**

- Click **Save**.
