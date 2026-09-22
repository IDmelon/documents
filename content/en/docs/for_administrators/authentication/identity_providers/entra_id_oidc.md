---
title: "Microsoft Entra ID (OIDC)"
description: "Integrate Microsoft Entra ID with IDmelon over OpenID Connect"
lead: ""
date: 2026-09-18T00:00:00+00:00
lastmod: 2026-09-22T00:00:00+00:00
draft: false
images: []
menu:
  docs:
    parent: "identity_providers"
weight: 25
toc: true
---

In this document you are going to set up **Entra ID** as an external IdP to IDmelon over OpenID Connect. IDmelon is the relying party.

This is not the SAML guide. SAML Entra ID is [Azure Active Directory (AAD)](../aad/). On the picker, use the **Entra ID** card, then choose **OpenID Connect**. Do not choose **SAML 2.0**, and do not open **Enterprise applications**.

Users are matched by `preferred_username` (the Entra UPN), not by the `email` claim. The Entra user must already exist in IDmelon with that same username.

## Initialize IDmelon Configuration as SP

Log in to the IDmelon panel. In the left menu open **Authentication**, then **External Identity Providers**.

Click **+ New Identity Provider**.

The next screen is **Select an App**. Under **Featured Applications**, click the **Entra ID** card. It shows both **OIDC** and **SAML** tags.

![Entra OIDC IDmelon 1](/images/vendor/sso/entra_id_oidc/entra_id_oidc_12.png)

**Choose Protocol** opens. Select **OpenID Connect**. The card is tagged **OIDC** and **Recommended**. Click **Select OIDC**. Do not click **Select SAML**.

![Entra OIDC IDmelon 2](/images/vendor/sso/entra_id_oidc/entra_id_oidc_13.png)

The wizard title is **Entra ID Integration**. The steps are **App Profile**, **OIDC Client Settings**, **Scopes and claims**, and **App Attributes Mapping**.

On **App Profile**, **Name** is already `Entra ID`. Leave it and click **Next**.

![Entra OIDC IDmelon 3](/images/vendor/sso/entra_id_oidc/entra_id_oidc_14.png)

**OIDC Client Settings** opens. Copy **Callback URL** with the copy button. You will paste that exact value as the Web redirect URI in Entra. The host depends on your environment. The path is `/api/oidc/sp/callback`. Leave this window open.

**Discovery URL** is prefilled as `https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration`. Do not click fetch yet. `{tenant}` is still in the URL, and the endpoint fields below are empty until you replace it.

![Entra OIDC IDmelon 4](/images/vendor/sso/entra_id_oidc/entra_id_oidc_15.png)

## Configuring Entra ID

Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com) or the [Azure portal](https://portal.azure.com) and open **App registrations** (`Home > App registrations`).

Click **+ New registration**.

![Entra OIDC Window 1](/images/vendor/sso/entra_id_oidc/entra_id_oidc_01.png)

On **Register an application**:

- **Name**: a display name, for example `IDmelon OIDC`.
- **Supported account types**: **Single tenant only**. The line shows your directory name.
- **Redirect URI**: in the platform dropdown choose **Web**, then paste the **Callback URL** copied from IDmelon.

Click **Register**.

![Entra OIDC Window 2](/images/vendor/sso/entra_id_oidc/entra_id_oidc_03.png)

The app **Overview** opens. Copy these two values. Do not click **Go to Enterprise applications**.

- **Directory (tenant) ID**. This replaces `{tenant}` in the IDmelon discovery URL.
- **Application (client) ID**. This is the IDmelon **Client ID**.

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
- Click the fetch button at the end of **Discovery URL**. IDmelon fills **issuer**, **authorization_endpoint**, **token_endpoint**, **jwks_uri**, and **userinfo_endpoint**. Do not type those by hand.
- Paste **Application (client) ID** into **Client ID**.
- Paste the secret **Value** into **Client secret**.
- Click **Next**.

On **Scopes and claims**:

- **Scopes**: leave `openid`, `profile`, and `email`. `"openid"` is always included.
- **Requested claims**: leave empty. Entra sends the claims used here with the scopes.

Click **Next**.

![Entra OIDC IDmelon 5](/images/vendor/sso/entra_id_oidc/entra_id_oidc_16.png)

On **App Attributes Mapping**, keep the one row and click **Confirm**:

| Claim | IDmelon user field |
| --- | --- |
| `preferred_username` | `username` |

![Entra OIDC IDmelon 6](/images/vendor/sso/entra_id_oidc/entra_id_oidc_17.png)

You return to **External Identity Providers**. The new row shows **Entra ID**, protocol **OIDC**, and status **Ready**.

![Entra OIDC IDmelon 7](/images/vendor/sso/entra_id_oidc/entra_id_oidc_18.png)

Open **Authentication > Authentication Profile** and select **External** in **Identity Provider**. Save if the control is enabled.

Users can then sign in through Entra ID. After they authenticate, Entra sends the browser to the callback URL you registered.
