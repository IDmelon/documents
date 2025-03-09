---
title: "Duo Admin"
description: ""
lead: ""
date: 2023-09-13T00:57:39+03:30
lastmod: 2025-02-05T14:57:39+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72300
toc: true
---

In this document you are going to set up `IDmelon` as an external IdP to the `Duo Admin`.

## Initialize IDmelon Configuration as IDP

---

Log in to the IDmelon panel, navigate to the **App Integration > Single Sign-On** section, and click **New Application**:

![IDmelon 1](/images/vendor/sso/duo_admin_sp/duo_admin_idmelon_panel_1.png)

Then select `Duo Admin` as service provider:

![IDmelon 2](/images/vendor/sso/duo_admin_sp/duo_admin_idmelon_panel_2.png)

You will need the values provided in the newly opened window to set up `Duo Admin` as SP:

![IDmelon 3](/images/vendor/sso/duo_admin_sp/duo_admin_idmelon_panel_3.png)

## Configuring Duo Admin as SP

---

To enable administrator SSO, you will need to log into your `Duo Admin panel`.

*Role required* : Owner

1. Log in to the [Duo Admin Panel](https://admin.duosecurity.com).
2. Click **Users** in the left sidebar, then click **Administrators**, finally click **Admin Login Settings**.

![alt](/images/vendor/sso/duo_admin_sp/duo_sp_01.png)

![alt](/images/vendor/sso/duo_admin_sp/duo_sp_02.png)

### Add Identity Provider

---

- Scroll to the **Single Sign-On with SAML Configuration** section of the "Administrator Login Settings" page.
- Enable SSO by changing the "Authentication with SAML" setting. The options for this setting are:
  - **Disabled**
  - **Optional**
  - **Required**

![alt](/images/vendor/sso/duo_admin_sp/duo_sp_03.png)

Selecting the **Optional** or **Required** SAML authentication option exposes the rest of the SSO configuration form. You'll need to enter below information about **IDmelon identity provider** in the **SAML Identity Provider Settings** section:

Identity provider: Custom Identity Provider

Configuration method: **Manual entry**

### Configure SAML 2.0 IdP

---

> Get all values for `{..}` from your IDmelon panel.
> If you are currently login here, you will see the replaced values instead.

- Entity ID or issuer ID: idp_issuer_uri
- Assertion consumer service URL or single sign-on URL: idp_single_sign_on_url
- Single logout URL: idp_single_sign_on_url
- Certificate: idp_certificate_download_url
- Encrypt assertions: Require unencrypted assertions
- Request signing: sign messages from Duo

![alt](/images/vendor/sso/duo_admin_sp/duo_sp_04.png)

- SHA-1 signatures: Uncheck
- Signed elements: Everything must be signed
- Click Save

![alt](/images/vendor/sso/duo_admin_sp/duo_sp_05.png)

### IDmelon SAML configuration

---

- You should Download **`metadata`** file (XML file) and **`copy values`** of below fields from this **Duo panel** to **IDmelon Panel**.

![alt](/images/vendor/sso/duo_admin_sp/duo_sp_06.png)

- Copy **Entity ID or Issuer ID**
- Copy **Assertion consumer service or single sign-on URL**
- You should copy values of above fields from **Duo panel** to **IDmelon Panel**.

## Finishing IDmelon Configuration as IDP

---

Provide required information in the `IDmelon` panel which was provided in the previous step.

![IDmelon 3](/images/vendor/sso/duo_admin_sp/duo_admin_idmelon_panel_1.png)
