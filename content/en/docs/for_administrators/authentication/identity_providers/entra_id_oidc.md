---
title: "Microsoft Entra ID (OIDC)"
description: "Integrate Microsoft Entra ID with IDmelon over OpenID Connect"
lead: ""
date: 2026-09-18T00:00:00+00:00
lastmod: 2026-09-18T00:00:00+00:00
draft: false
images: []
menu:
  docs:
    parent: "identity_providers"
weight: 25
toc: true
---

In this document you are going to set up `Entra ID` as an external IdP to IDmelon over OpenID Connect. IDmelon is the relying party.

This is not the SAML guide. SAML Entra ID is [Azure Active Directory (AAD)](../aad/). On the picker, use the **Entra ID** card tagged **OIDC**. Do not use the **Entra ID** card tagged **SAML 2.0**, and do not open **Enterprise applications**.

## Initialize IDmelon Configuration as SP

Log in to the IDmelon panel. In the left menu open **Authentication**, then **External Identity Providers**.

Click **+ New Identity Provider**.

![Entra OIDC IDmelon 1](/images/vendor/sso/entra_id_oidc/entra_id_oidc_11.png)

The next screen is **Select an App**. Under **Featured Applications**, click the **Entra ID** card whose tag is **OIDC**.

![Entra OIDC IDmelon 2](/images/vendor/sso/entra_id_oidc/entra_id_oidc_12.png)

The wizard title is **Entra ID (OIDC) Integration**. The steps are **App Profile**, **OIDC Client Settings**, **Scopes**, and **App Attributes Mapping**.

On **App Profile**, **Application Name** is already `Entra ID`. Leave it and click **Next**.

![Entra OIDC IDmelon 3](/images/vendor/sso/entra_id_oidc/entra_id_oidc_14.png)

**OIDC Client Settings** opens. Copy **Callback URL** with the copy button. You will paste that exact value as the Web redirect URI in Entra. The host depends on your environment. The path is `/api/sso/oidc/sp/callback`. Leave this window open.

**Discovery URL** is prefilled as `https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration`. Do not click fetch yet. `{tenant}` is still in the URL, and the endpoint fields below are empty until you replace it.

![Entra OIDC IDmelon 4](/images/vendor/sso/entra_id_oidc/entra_id_oidc_15.png)

## Configuring Entra ID

Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com) and open **App registrations** (`Home > App registrations`).

Click **+ New registration**.

![Entra OIDC Window 1](/images/vendor/sso/entra_id_oidc/entra_id_oidc_01.png)

On **Register an application**:

- **Name**: a display name, for example `IDmelon OIDC`.
- **Supported account types**: **Single tenant only**. The line shows your directory name.
- **Redirect URI**: in the platform dropdown choose **Web**, then paste the **Callback URL** copied from IDmelon. A green check means the URI format is accepted.

Click **Register**.

![Entra OIDC Window 2](/images/vendor/sso/entra_id_oidc/entra_id_oidc_03.png)

The app **Overview** opens. Copy these two values. Do not click **Go to Enterprise applications**.

- **Application (client) ID**. This is the IDmelon **Client ID**.
- **Directory (tenant) ID**. This replaces `{tenant}` in the IDmelon discovery URL.

**Redirect URIs** should read **1 web**. That is the callback you just registered.

![Entra OIDC Window 3](/images/vendor/sso/entra_id_oidc/entra_id_oidc_04.png)

In the left menu, under **Manage**, open **Certificates & secrets**. Open the **Client secrets** tab and click **+ New client secret**.

In **Add a client secret**:

- **Description**: any label for this secret.
- **Expires**: pick a lifetime, for example **730 days (24 months)**.

Click **Add**.

![Entra OIDC Window 4](/images/vendor/sso/entra_id_oidc/entra_id_oidc_05.png)

The new row appears under **Client secrets**. Copy the **Value** column immediately. Entra hides it after you leave the page. That value is the IDmelon **Client secret**. Do not copy **Secret ID**.

![Entra OIDC Window 5](/images/vendor/sso/entra_id_oidc/entra_id_oidc_07.png)

## Finishing IDmelon Configuration as SP

Return to **OIDC Client Settings**.

- In **Discovery URL**, replace `{tenant}` with the **Directory (tenant) ID** you copied. You can use the tenant domain instead, such as `contoso.onmicrosoft.com`. Fetch stays blocked while `{tenant}` is still in the URL.
- Click the fetch button at the end of **Discovery URL**. IDmelon fills **Authorization endpoint**, **Token endpoint**, **JWKS URI**, and **UserInfo endpoint**. Do not type those by hand.
- Paste **Application (client) ID** into **Client ID**.
- Paste the secret **Value** into **Client secret**.
- Click **Next**.

On **Scopes**, leave `openid`, `profile`, and `email`. The list is locked. Click **Next**.

On **App Attributes Mapping**, keep `email` → `email` and click **Confirm**. The Entra user must already exist in IDmelon with that email.

For that claim to be in the ID token, open the app registration, go to **Token configuration**, click **Add optional claim**, choose token type **ID**, check `email`, and add it. If Entra asks to turn on the Microsoft Graph `email` permission, turn it on.

Open **Authentication > Authentication Profile** and select **External** in **Identity Provider**.

Users can then sign in through Entra ID. After they authenticate, Entra sends the browser to the callback URL you registered.
