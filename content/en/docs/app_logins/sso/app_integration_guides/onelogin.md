---
title: "OneLogin"
description: "How to integrate IDmelon with OneLogin for single sign-on."
lead: ""
date: 2023-09-13T00:58:26+03:30
lastmod: 2023-09-13T00:58:26+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72700
toc: true
---

> **⚠️ Important Notice**
>
> The content in this document is currently under review and will be updated soon. We are actively working on providing you with the most accurate and up-to-date information. Thank you for your patience and understanding.

In this document you are going to set up `IDmelon` as an external IdP to the `OneLogin`.

## Login to OneLogin administration panel

---

- **Login to** [OneLogin](https://onelogin.com).
- **Open** OneLogin dashboard.
- On the right side of navbar click on**Administration**

![alt](/images/vendor/sso/onelogin_sso.png)

## Add Identity Provider

---

- Tab **then click on Trusted idps**
- Click on **New Trust**

![alt](/images/vendor/sso/onelogin_sso1.png)

- **Choose** a custom name at the left side of page
- **Enable** Trusted IDP, for this action certificate validation is necessary
- **Enable** Show in Login panel, **Choose** Custom logo
- **IDmelon Logo Domain :**

![alt](https://uploads-ssl.webflow.com/63c6ee71910e8ac258ffc84e/652f86c1521cc726e660b767_IDmelon-Logo-213x60.svg)

> Get all values for `{..}` from your IDmelon panel.
> If you are currently login here, you will see the replaced values instead.

- **Issuer** : Show in Login panel, idp\_issuer\_uri
- **Email Domains** : The Email Domains field is used to automatically invoke this Trusted IdP when a user enters their email address at login time - if the email address is unrecognized, but belongs to one of the domains listed, then this TIdP will be invoked via an authentication request (SAML, OIDC or OAuth as appropriate), **Example** = idmelon.com

![alt](/images/vendor/sso/onelogin_sso2.png)

- To enable Standard mode, check Sign users into OneLogin. This allows inbound identities from the Identity Provider to be matched to local user accounts within the tenant, via responses to the /access/idp endpoint.
- To send the user identity within the authentication request sent to the Trusted Identity Provider, check Send Subject Name ID or Login Hint in Auth Request: if the Trusted IdP is configured to use SAML, then the authentication request is sent as a Subject NameID parameter whilst if OIDC or OAuth is used, the same information is sent as a query string parameter called login\\\_hint. This feature is to provide an improved user experience by avoiding the need for the user to provide an identifier to both OneLogin and the Trusted IdP.
- Sign users into OneLogin
- Send Subject Name ID or Login Hint in Auth Request
- SAML - hard-coded to extract the SAML Subject NameID. It can't be changed.
- **User Attribute Mapping** : Email

![alt](/images/vendor/sso/onelogin_sso3.png)

> Get all values for `{..}` from your IDmelon panel.
> If you are currently login here, you will see the replaced values instead.

- **IdP Login URL :** idp\_single\_sign\_on\_url
- **IdP Logout URL :** idp\_single\_sign\_on\_url
- **IdP Issuer URI** idp\_issuer\_uri

> Note : please save the configuration then scroll it to continue with certificate part.

## IDmelon SAML configuration

---

You should copy values of below fields from the Okta panel to the IDmelon Panel:

- **entity id: Copy** SP Entity ID
- **single log out: Copy** SP logout url
- **Assertion Consumer Service URL :** `https://{your custom subdomain}.onelogin.com/access/idp`

![alt](/images/vendor/sso/onelogin_sso4.png)

- **Choose :** X.509 Certificate = Standard Strength Certificate (2048-bit)
- **Choose :** X.509 Certificate = Standard Strength Certificate (2048-bit)
- **Click on** View Details
- **Choose :** SHA256
- **Download :** X.509 Certificate

![alt](/images/vendor/sso/onelogin_cert.png)

- Click on **Save**
- Trusted IdP Certificate :`idp_certificate_download_url`
- **JIT :** Enable, Set User TIDP after user creation

![alt](/images/vendor/sso/onelogin_jit.png)

- Enable
- Set User TIDP after user creation
- Click on **Save**.

## Enable SSO

---

- Click on **More Actions** then click on **Set as default Trusted Idp**.
- this option will activate sso for **all users.**.

![alt](/images/vendor/sso/sso_onelogin.png)

- **OR Enable sso** for special users.
- Click on special**user** then Click on **Authentication**.
- Choose **Custom Trusted IDP**.

![alt](/images/vendor/sso/sso_onelogin1.png)

- Click on **Save**.

## API Token

---

### GET API Token for user provisioning in OneLogin

- Click on **Developers** then **API Credentials**.
- Click on **New Credential** .

![alt](/images/vendor/sso/onelogin_api.png)

- Choose Custom **Name**.
- Click on **Manage all**.

![alt](/images/vendor/sso/onelogin_api1.png)

- **Save**.

**Custom name API Copy** Client ID

- Copy **Client Secret**.

![alt](/images/vendor/sso/onelogin_api2.png)

- **Done**.
