---
title: "Google"
description: "How to integrate IDmelon with Google Workspace for single sign-on."
lead: ""
date: 2023-09-13T00:58:07+03:30
lastmod: 2023-09-13T00:58:07+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72500
toc: true
---

In this document you are going to set up `IDmelon` as an external IdP to the `Google`.

## Login to Google administration panel

---

In order to set up the connection, you will need to log into your [Google administration panel](https://admin.google.com).

## Add Identity Provider

---

In the `Google administration panel`, from the side menu, navigate to the `Security` menu.

Then from the `Security` menu, select the `Overview` sub-menu.

![alt](/images/vendor/sso/google_dashboard_01.png)

Then scroll down and navigate to `Set up Single Sign-On (SSO) with a third-party IdP`

![alt](/images/vendor/sso/google_dashboard_02.png)

- In **Third-party SSO profile for your organization**, click **Add SSO profile**.
- Check the **Set up SSO with third-party identity provider** box.

> Get all values for {..} from your IDmelon panel.
> If you are currently logged in here, you will see the replaced values instead.

- **Sign-in page URL**: idp_single_sign_on_url
- **Sign-out page URL**: idp_single_sign_on_url
- **Verification Certificate**: idp_certificate_download_url
- Click **Save**.

![alt](/images/vendor/sso/google_dashboard_03.png)

### Create an SSO profile

- In the **Single Sign-On (SSO) with third-party Identity Providers (IDPs)** panel.
- Scroll down to **Third-party SSO profiles**, click **Add SAML profile**.

![alt](/images/vendor/sso/google_dashboard_04.png)

- Enter a name for the profile.
- **IDP entity ID**: idp_issuer_uri
- **Sign-in page URL**: idp_single_sign_on_url
- **Upload certificate**: idp_certificate_download_url
- Click **Save**.

![alt](/images/vendor/sso/google_dashboard_05.png)

## IDmelon SAML configuration

---

After completing the previous `Setup Google connection` section and submitting the form,  you can see a new IdP added to your list.

You should copy values of below fields from the `SP details` Under the `SAML SSO profile` to the `IDmelon` Panel:

- **Entity ID**
- **ACS URL**

![alt](/images/vendor/sso/google_dashboard_06.png)

## Manage SSO profile assignments

---

Scroll down and navigate to `Manage SSO profile assignments`.

1. Choose **Another SSO Profile**.
2. Select your Custom SSO config **Select SSO profile**.
3. Choose **Have Google prompt for their username, then redirect them to this profile's IDP sign-in page.**.

You can choose `Users`, `Groups`, `Organizational units` to activate IDmelon SSO.

![alt](/images/vendor/sso/google_dashboard_07.png)

## Domain-specific service URLs

---

Scroll down and navigate to `Domain-specific service URLs`.

![alt](/images/vendor/sso/google_dashboard_08.png)

Check `Automatically redirect users to the third-party IdP in the following SSO profile` box, To finally submit SSO profile.

Then Select `SSO Profile for your organization`, under the `SSO profile` field.

Click `Save`.

![alt](/images/vendor/sso/google_dashboard_09.png)

## API Token

---

- In the [Google Cloud console](https://console.cloud.google.com/apis), enable **the People API**.
- Create **New Project** :
  - Choose a custom name in **Project name**
  - Choose an organization in **Location**
  - Click **CREATE**

![alt](/images/vendor/sso/google_dashboard_10.png)

- Select your Custom **Project**.
- **Confirm project**, then click **NEXT**.

![alt](/images/vendor/sso/google_dashboard_11.png)

- Click **ENABLE**.

![alt](/images/vendor/sso/google_dashboard_12.png)

- Then from **Google Console**, in **APIs and services panel**, click **CREATE CREDENTIALS** and choose each Api option which you want to use.

![alt](/images/vendor/sso/google_dashboard_13.png)

- Then the API will be created, and you can copy your needed values:
  - If you choose OAuth client ID.

![alt](/images/vendor/sso/google_dashboard_14.png)
