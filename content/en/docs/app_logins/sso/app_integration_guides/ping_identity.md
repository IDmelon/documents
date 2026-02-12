---
title: "Ping Identity"
description: "How to integrate IDmelon with Ping Identity for single sign-on."
lead: ""
date: 2023-09-13T00:58:44+03:30
lastmod: 2023-09-13T00:58:44+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72800
toc: true
---

> **⚠️ Important Notice**
>
> The content in this document is currently under review and will be updated soon. We are actively working on providing you with the most accurate and up-to-date information. Thank you for your patience and understanding.

In this document you are going to set up `IDmelon` as an external IdP to the `Ping Identity`.

## Login to Ping Identity administration panel

---

- **Login to** [**Ping Identity**](https://www.pingidentity.com/en/account/sign-on.html).
- Open Ping Identity dashboard.
- On the right side of navbar Click on **Administrators**

![alt](/images/vendor/sso/ping_dashboard1.png)

## Add Identity Provider

---

- Click **Connection**
- Under IDENTITY PROVIDERS Click on **External IdPs**

![alt](/images/vendor/sso/ping_dashboard2.png)

- Click on **Add Provider**

![alt](/images/vendor/sso/ping_dashboard3.png)

- Click on **SAML**

![alt](/images/vendor/sso/ping_dashboard4.png)

## Configure SAML 2.0 IdP

---

- Configure SAML 2.0 IdP
  - Create IdP Profile
  - **NAME** : Custom\_name
  - **DESCRIPTION** : Custom\_description
  - **ICON** : Custom\_icon
  - **LOGIN BUTTON** : Custom\_login\_button
  - **Continue**
  - Configure PingOne Connection then select **Continue**

![alt](/images/vendor/sso/ping_dashboard5.png)

- Configure IdP Connection
- Choose : **Manually enter the following metadata information**

> Get all values for {..} from your IDmelon panel.
> If you are currently login here, you will see the replaced values instead.

- **SSO endpoint :** idmelon single sign on url
- **IdP entity ID :** idp\_issuer\_uri
- **SSO binding :** Specifies the binding to use for the authentication request. Select HTTP Post or HTTP Redirect.
- **VERIFICATION CERTIFICATE :** idp\_certificate\_download\_url

![alt](/images/vendor/sso/ping_dashboard6.png)

- **Save and Continue**
- On the **Map Attributes** page, define how the PingOne user attributes are mapped to identity provider attributes. For more information, see [Mapping attributes](https://docs.pingidentity.com/bundle/pingone/page/pwv1567784207915.html)
  - Enter the PingOne user profile attribute and the external IdP attribute. For more information about attribute syntax, see [Identity provider attributes](https://docs.pingidentity.com/bundle/pingone/page/qtp1647276356200.html)
  - To add an attribute, click **\\+ Add attribute**
  - To use the expression builder, click **Build and test** or **Advanced Expression**. See [Using the expression builder](https://docs.pingidentity.com/bundle/pingone/page/hri1641569602468.html)
  - Select the update condition, which determines how PingOne updates its user directory with the values from the identity provider. The options are:
    - **Empty only**: Update the PingOne attribute only if the existing attribute is empty.
    - **Always**: Always update the PingOne directory attribute.

![alt](/images/vendor/sso/ping_dashboard8.png)

- **Save & Finish**
- Enabling or disabling IDmelon as Identity Provider : [Reference](https://docs.pingidentity.com/bundle/pingone/page/hvt1567784210895.html)

![alt](/images/vendor/sso/ping_dashboard9.png)

- **Finish**

## Setting idp-init for Ping Identity

---

- According to [Ping Identity document](https://docs.pingidentity.com/bundle/pingone/page/wou1649186902011.html)
- Go to **Connections** → **Applications**
- Add a **new Oidc Application**

![alt](/images/vendor/sso/ping_idp_init1.png)

- Add Application
  - Choose a custom application Name
  - Choose a custom application Description
  - Choose a custom application ICON
  - Choose OIDC Web App
  - Save

![alt](/images/vendor/sso/ping_idp_init2.png)

- Go to **Oidc app** which created
- Activate **Oidc app** which on the rightside
- Go to **Configuration** then click on **Icon edit**

![alt](/images/vendor/sso/ping_idp_init3.png)

- Go to **Connections** → **Applications**
- For **Admins** IdP init Login
  - Click on **PingOne Admin Console**
  - Copy **Home Page URL**

  ![alt](/images/vendor/sso/ping_idp_init4.png)

  - Done

  For **users** IdP init Login:

  - Click on **PingOne Application Portal**
  - Copy **Home Page URL**

  ![alt](/images/vendor/sso/ping_idp_init5.png)

  - Done

- Paste **Home Page URL** to Initiate Login URI field
- Paste **Home Page URL** to Target Link URI field

![alt](/images/vendor/sso/ping_idp_init6.png)

- Click the **Overview tab.**
- Locate the **Client ID** and copy it to a relaystate field of IDmelon.
- **Example** : Relaystate = applicationId=Custom Client ID

![alt](/images/vendor/sso/ping_idp_init7.png)

## IDmelon SAML configuration

---

- Go to**Connections** → **External IdPs**
- Go to **IDmelon** IdP then click on **IdP Configuration** click **edit**

![alt](/images/vendor/sso/ping_metadata.png)

- Download -> **DOWNLOAD METADA** File
  - You should upload **metadata** file and **copy values** of below fields from this **Ping Identity panel** to **IDmelon Panel**.

![alt](/images/vendor/sso/ping_metadata2.png)

- Go to **IDmelon IdP** then click on **P1 Connection** click **edit**
  - Copy -> **PINGONE (SP) ENTITY ID**
  - Select your **SIGNING CERTIFICATE**
  - Select **RSA\_SHA256** in **SIGNING ALGORITHM** field
    - You should copy values of below fields from this **Ping Identity panel** to **IDmelon Panel**.

![alt](/images/vendor/sso/ping_dashboard_10.png)

## Passwordless

---

1. Open **Admin dashboard** of Ping Identity
2. Click on **Experiences** then **Authentication Admin dashboard** of Ping Identity
3. Under **Authentication Policies**
4. **Choose or create**your custom Authentication
5. **example :**
    - Click on **PRESENTED IDENTITY PROVIDERS**
    - Choose **IDmelon as Identity Provider**
    - **Save**

![alt](/images/vendor/sso/ping_dashboard7.png)

## API Token

---

- Click on **Connections** then **Applications**
- Click on **Add Applications** then **Choose Worker**
- Choose **Custom name**
- Optional : **Custom description**
- Click on **Save**

![alt](/images/vendor/sso/ping_api.png)

- Click on **Worker** hen Click on **Configuration**
- Scroll down then Click on **Get Access Token**
- Copy **Access Token** click on **Done**

![alt](/images/vendor/sso/ping_api1.png)

> **Note :** Activate Worker Application

- **Done**
