---
title: "Duo End-user"
description: "How to integrate IDmelon with Duo for end-user single sign-on."
lead: ""
date: 2023-09-13T00:57:56+03:30
lastmod: 2023-09-13T00:57:56+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72400
toc: true
---

> **⚠️ Important Notice**
>
> The content in this document is currently under review and will be updated soon. We are actively working on providing you with the most accurate and up-to-date information. Thank you for your patience and understanding.

In this document you are going to set up `IDmelon` as an external IdP to the `Duo-user`.

## Login to Duo administration panel

---

- Log in to the [Duo Admin Panel](https://admin.duosecurity.com).

## Add Identity Provider

---

- Click **Single Sign-On** in the left sidebar, and then if **active Required** click active and start then click **Add Saml Identity Provider**.

![alt](/images/vendor/sso/duo_user-sso.png)

## IDmelon SAML configuration

---

**You should copy values of below fields from the Duo panel to the IDmelon Panel.**

- **Copy** Entity ID Value
- **Copy** Assertion Consumer Service URL

![alt](/images/vendor/sso/duo_user-sso1.png)

## Configure Duo Single Sign-On

---

- **SAML Identity Provider Configuration**

  > Get all values for `{..}` from your IDmelon panel.
  > If you are currently login here, you will see the replaced values instead.

  1. **Choose** Custom Display Name
  2. **Paste Entity ID :** idp_issuer_uri
  3. **Paste Single Sign-On URL :** idp_single_sign_on_url
  4. **Paste Single Single Logout URL :** idp_single_sign_on_url
  5. **Paste Single Logout Redirect URL :** <https://duo.com>
  6. **Import Certificate :** idp_certificate_download_url
  7. **Choose Username normalization :** Simple

![alt](/images/vendor/sso/duo_user-sso2.png)

- Click Save
- Click **Duo Central** in the left sidebar, and then if **active Required** click active and start.

## Passwordless

---

- Click **Configuration & Policy**
  - **Activate :** Status to **Online**
  - **Choose :** Duo Central name
  - **Create your custom subdomain** or **Use** default

![alt](/images/vendor/sso/duo_user-sso3.png)

### Add IDmelon to Duo-user as passwordless and/or one Factor access

- **Scroll down** to policy options Click **Edit Global Policy**

![alt](/images/vendor/sso/duo_user-sso4.png)

- Click **Authentication policy** Choose **Bypass 2FA**

![alt](/images/vendor/sso/duo_user-sso5.png)

- Click **Authentication methods** then **disable all options of 2FA methods**.

![alt](/images/vendor/sso/duo_user-sso6.png)

- **Click** Save Policy

## API Token

---

- **Go to** Applications
- **Click on** Protect an Applications

![alt](/images/vendor/sso/duo_admin.png)

- **Search** for Admin API & **Click on** Protect

![alt](/images/vendor/sso/duo_admin1.png)

- **Copy values** for Protect:
    1. Integration key
    2. Secret key
    3. API hostname

![alt](/images/vendor/sso/duo_admin2.png)

- **Click on** Save Changes

## Add Application

---

- Click **Tiles** to add applications to your Duo-user dashboard
  - Add **tile**

![alt](/images/vendor/sso/duo_user-sso7.png)

- **Choose** to Add **Applications** or Add **Bookmarks**
- **Example**: Add Application tile
  - **Choose** your custom app

![alt](/images/vendor/sso/duo_user-sso8.png)

- **Click** Add tile
- **All Done.**

### IDP-Init/SP-Init support

---

Note that Duo-user doesn't support IDP-Init

![alt](/images/vendor/sso/duo-user-idp-init.png)
