---
title: "Azure Active Directory (AAD)"
description: "Integrate Azure Active Directory (AAD) with IDmelon"
lead: ""
date: 2025-02-05T09:15:00+03:30
lastmod: 2025-02-05T09:15:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "identity_providers"
weight: 20
toc: true
---

In this document you are going to set up `AAD` as an external IdP to the IDmelon.

## Initialize IDmelon Configuration as SP

Log in to the IDmelon panel, navigate to the **Authentication** section, and click **Integrate with a New Identity Provider**:

![AAD IDmelon 1](/images/vendor/sso/aad/aad_11.png)

Then select **Entra ID** as identity provider:

![AAD IDmelon 2](/images/vendor/sso/aad/aad_12.png)

You will need the values provided in the newly opened window to set up **Entra ID** as an external IDP:

![AAD IDmelon 3](/images/vendor/sso/aad/aad_13.png)

## Configuring AAD as external IDP

- Login to the admin azure panel from [here](https://portal.azure.com/#home) , and click the `Enterprise applications`.

![AAD Window 1](/images/vendor/sso/aad/aad_01.png)

- From the `All applications` menu, click to the `New application` .

![AAD Window 2](/images/vendor/sso/aad/aad_02.png)

- Click on the `Create your own application`.

![AAD Window 3](/images/vendor/sso/aad/aad_03.png)

- Provided a `name` for your application.

- Select `Non-gallery` option.

![AAD Window 4](/images/vendor/sso/aad/aad_04.png)

- Click the `Get started` of the `Set up single sign on` section.

![AAD Window 5](/images/vendor/sso/aad/aad_05.png)

- Click the `SAML` section.

![AAD Window 6](/images/vendor/sso/aad/aad_06.png)

- Click `Edit` option of the `Basic SAML Configuration` section.

![AAD Window 7](/images/vendor/sso/aad/aad_07.png)

- Click `Add identifier` and fill it from **IDmelon Authentication Panel**.

- Click `Add reply URL` and fill it from **IDmelon Authentication Panel**.

- fill `Logout URL` from **IDmelon Authentication Panel**.

![AAD Window 8](/images/vendor/sso/aad/aad_08.png)

## Finishing IDmelon Configuration as SP

- Scroll down and copy the values from **Entra ID** to the **IDmelon Authentication Panel** as shown in the images.

![AAD Window 9](/images/vendor/sso/aad/aad_09.png)
![AAD IDmelon 3](/images/vendor/sso/aad/aad_14.png)
