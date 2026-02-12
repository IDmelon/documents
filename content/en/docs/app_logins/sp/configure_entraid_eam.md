---
title: "Configure Entra ID EAM"
description: "Configuring IDmelon as Entra ID External Authentication Methods"
lead: ""
date: 2023-12-15T23:05:00+03:30
lastmod: 2025-02-04T16:39:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "app_integrations"
weight: 15
toc: true
---

IDmelon can be inegrated to work as [extenal authentication method in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/authentication/how-to-authentication-external-method-manage).

## Microsoft Entra ID

In this document you are going to set up **IDmelon** as an external IdP to **Microsoft** external authentication method _(EAM)_.

### Prerequisites

- An active **Entra ID P1 or P2 subscription** including Conditional Access, with the P1/P2 licenses assigned to each user that will log in using IDmelon 2FA.

- A designated Entra ID admin service account to use for authorizing the IDmelon application access. This account needs the Entra ID **Global Administrator** or **Privileged Role Administrator** role during IDmelon setup.

### Setup IDmelon as IDP

- Log in to the **IDmelon** panel, navigate to the `App Integration > 2FA Protection`, and click `New Application`:
</br>

![image1](/images/vendor/sso/eam/1.png)
</br>

- You will be needing the values provided in the newly opened window to setup **IDmelon** as external IDP:
</br>

![image2](/images/vendor/sso/eam/2.png)
</br>

### Create New Application In Azure

- Login to the admin azure panel from [here](https://portal.azure.com/#home) , and click the `Enterprise applications`.
</br>

![image3](/images/vendor/sso/eam/3.png)
</br>

- From the `All applications` menu, click on the `New application`.
</br>

![image4](/images/vendor/sso/eam/4.png)
</br>

- Click on the `Create your own application`.
</br>

![image5](/images/vendor/sso/eam/5.png)
</br>

- Provided a `name` for your application.
- Select `App you're developing` option.
- Click on `Create`.
</br>

![image6](/images/vendor/sso/eam/6.png)
</br>

- Select `Multitenant` option.
- Select `Web` platform and paste `Redirect URI` provided in **IDmelon** panel.
- Click on `Register`
</br>

![image7](/images/vendor/sso/eam/7.png)
</br>

- Go back to `Enterprise Applications` and click on your newly created application _(Application ID will be needed)_.
</br>

![image7-a](/images/vendor/sso/eam/7-a.png)
</br>

![image7-b](/images/vendor/sso/eam/7-b.png)
</br>

### Configure Entra ID To Use IDmelon As External IDP

- Go to the `Microsoft Entra ID` and from left bar navigate to `Manage > Security`.
</br>

![image8](/images/vendor/sso/eam/8.png)
</br>

- In newly opened widow navigate to `Manage > Authentication Methods`.
</br>

![image9](/images/vendor/sso/eam/9.png)
</br>

- Click on `Add external method`.
</br>

![image10](/images/vendor/sso/eam/10.png)
</br>

- Provide a `name` to be displayed on the **Microsoft** MFA page.
- Fill in the `Client ID` field with value Provided by **IDmelon**.
- Fill in the `Discovery Endpoint` field with value Provided by **IDmelon**.
- Fill in the `App ID` field with your **Entra ID** application ID.
- Provide admin consent.
- Click on `Enable` button.
- Click on `Save`.
</br>

![image11](/images/vendor/sso/eam/11.png)
</br>

> **Note:** If the `Request admin consent` information shows a Request permission button instead of saying `Admin consent granted`, click the Request permission button to authorize the grant the IDmelon External Authentication Method application, making sure to check the box next to Consent on behalf of your organization before clicking Accept.

### Create And Apply IDmelon Conditional Access Policy

- Return to `Enterprise Applications` and click on your newly created application.
- Navigate to `Security > Conditional Access`
</br>

![image12](/images/vendor/sso/eam/12.png)
</br>

- Click on `New Policy`
</br>

![image13](/images/vendor/sso/eam/13.png)
</br>

- Provide a name for policy.
- Click on `Users` and apply this policy to the desired users.
</br>

![image14](/images/vendor/sso/eam/14.png)
</br>

- Click on `Target resources` and apply this policy to the desired resources.
</br>

![image15](/images/vendor/sso/eam/15.png)
</br>

- Click on `Grant`.
- Select `Grant access`.
- Check `Require multifacore authentication` box.
- Click on `Select`.
</br>

![image16](/images/vendor/sso/eam/16.png)
</br>

- Enable the policy.
- Click on `Create`.
</br>

![image17](/images/vendor/sso/eam/17.png)
</br>
