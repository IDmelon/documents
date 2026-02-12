---
title: "Salesforce"
description: "How to integrate IDmelon with Salesforce for single sign-on."
lead: ""
date: 2023-09-13T00:59:07+03:30
lastmod: 2023-09-13T00:59:07+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72900
toc: true
---

> **⚠️ Important Notice**
>
> The content in this document is currently under review and will be updated soon. We are actively working on providing you with the most accurate and up-to-date information. Thank you for your patience and understanding.

In this document you are going to set up `IDmelon` as an external IdP to the `Salesforce`.

## Login to  Salesforce administration panel

---

In order to set up the connection, you will need to log into to your `Salesforce administration panel`.

To login to the `Salesforce administration panel`, click the `Setting` icon on the top right corner of Salesforce user panel page then Click `Setup`.

![alt](/images/vendor/sso/salesforce_dashboard_01.png)

## Add Identity Provider

---

In the `Salesforce administration panel`, from the side menu, navigate to the `Security` menu, then Click `identity` and finally select `Single Sign-On Settings`.

![alt](/images/vendor/sso/salesforce_dashboard_02.png)

Then Check `SAML Enable` box from the main panel.
Under `Single Sign-On Settings` Click New.

![alt](/images/vendor/sso/salesforce_dashboard_03.png)

## Configure SAML 2.0 IdP

---

To configure SAML 2.0 IdP, fill the values of settings as below.

- Click **Name:** Your custom name
- **API Name:** Your custom api name

> Get all values for {..} from your IDmelon panel.
> If you are currently login here, you will see the replaced values instead.

- **Issuer:** `{{idp_issuer_uri}}`
- **Entity ID:** `Your custom salesforce portal`
  - **Example:** https://idmelon\_test\_sso.my.salesforce.com
- **IdP Signature Certificate :**`{{idp_certificate_download_url}}`
- **Request Signing Certificate :**`Your salesforce certificate`
- **Request Signature Method :**`RSA_SHA256`
- **Assertion Decryption Certificate :** `Assertion not encrypted` **or** `Your salesforce certificate`
- **SAML Identity Type :** `SAML Identity Type`
- **SAML Identity Location :**`Identity is in the NameIdentifier element of the Subject statement`
- **Service Provider Initiated Request Binding : HTTP POST**
- **Identity Provider Login URL :**`{{idp_single_sign_on_url}}`
- **Custom Logout URL :**`{{idp_single_sign_on_url}}`
- Click **Save.**

![alt](/images/vendor/sso/salesforce_dashboard_04.png)

## IDmelon SAML configuration

---

After completing previous `SAML Single Sign-On Settings` section and submitting the form, you can see a new IdP added to your list.

You should upload **`metadata`** file from this **Salesforce panel** to **IDmelon Panel**.

- Download -> Download Metadata File

You should copy values of below fields from the `Salesforce` panel to the `IDmelon` Panel:

- **Entity ID**`Your custom salesforce portal`
  - **Example** https://idmelon_test_sso.my.salesforce.com
- **Login URL :** Copy and Paste to **Assertion consumer service url**
- **Logout URL :** Copy and Paste to **Single Sign Logout url**

![alt](/images/vendor/sso/salesforce_dashboard_05.png)

## Add IDmelon to Salesforce Login Panel

In the `Salesforce administration panel`, from the side menu, navigate to the `Company Settings` menu, then Click `My Domain`.

![alt](/images/vendor/sso/salesforce_dashboard_06.png)

Scroll down to `Authentication Configuration` menu, then Click `Edit`.

![alt](/images/vendor/sso/salesforce_dashboard_07.png)

Under `Authentication Configuration` Dashboard, Check your custom IDP from `Authentication Service` check boxes.

![alt](/images/vendor/sso/salesforce_dashboard_08.png)

- **Done**
