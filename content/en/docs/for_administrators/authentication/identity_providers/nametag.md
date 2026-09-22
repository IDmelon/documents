---
title: "Nametag"
description: "Integrate Nametag with IDmelon over OpenID Connect"
lead: ""
date: 2026-09-18T00:00:00+00:00
lastmod: 2026-09-22T00:00:00+00:00
draft: false
images: []
menu:
  docs:
    parent: "identity_providers"
weight: 30
toc: true
---

In this document you are going to set up **Nametag** as an external IdP to IDmelon. IDmelon is the relying party. Use the **Nametag** tile marked **OIDC**. Do not use a SAML tile, and do not use **+ Add Custom Configuration**.

Nametag verifies the person with a government ID. This flow does not use email: there is no `email` scope, and matching is by verified first and last name.

## Initialize IDmelon Configuration as SP

Log in to the IDmelon panel. In the left menu open **Authentication**, then **External Identity Providers**.

Click **+ New Identity Provider**.

![Nametag IDmelon 1](/images/vendor/sso/nametag/nametag_11.png)

The next screen is **Select an App**. Under **Featured Applications**, click the **Nametag** card. Its tag is **OIDC**. Other cards (for example **Entra ID** with **OIDC** / **SAML**, or **Okta** with **SAML**) are different integrations.

![Nametag IDmelon 2](/images/vendor/sso/nametag/nametag_12.png)

The wizard title is **Nametag Integration**. The steps are **App Profile**, **OIDC Client Settings**, **Scopes and claims**, and **App Attributes Mapping**.

On **App Profile**, **Name** is already `Nametag`. Leave it and click **Next**.

![Nametag IDmelon 3](/images/vendor/sso/nametag/nametag_14.png)

**OIDC Client Settings** opens. Copy **Callback URL** with the copy button. You will add that exact value in the Nametag console. The host depends on your environment. The path is `/api/oidc/sp/callback`. Leave this window open.

**Discovery URL** is already `https://nametag.co/.well-known/openid-configuration`. Click the fetch button at the end of that field. IDmelon fills:

- **Issuer**: `https://nametag.co`
- **authorization_endpoint**: `https://nametag.co/oauth2/authorize`
- **token_endpoint**: `https://nametag.co/oauth2/token`
- **jwks_uri**: `https://nametag.co/.well-known/jwks`

Do not type the endpoints by hand.

**Client ID** and **Client secret** stay empty until you copy them from Nametag. You will paste them after the Nametag steps below, then continue.

![Nametag IDmelon 4](/images/vendor/sso/nametag/nametag_15.png)

## Configuring Nametag

Sign in at [console.nametag.co](https://console.nametag.co). Open the environment you will use. A Sandbox environment shows a banner that the brand is not verified and that requests are not billed. Production does not show that banner.

In the left sidebar, under **Environment settings**, open **OAuth**.

![Nametag Window 1](/images/vendor/sso/nametag/nametag_01.png)

On this page set three things. Do not use **Organization settings > API keys**. That key is not the OAuth client secret.

### Client ID

Copy **Client ID** with the **Copy** button. This is the public part of the API credentials. Paste it into IDmelon **Client ID**.

### Client Secret

If the table already has a key and its **Enabled** switch is on, you can reuse it only if you still have the full secret. The table shows a prefix, not the secret.

Otherwise click **Create new API key**. Keep the default name, create the key, and copy the full value before you leave the dialog. Nametag shows it only once. That value is the IDmelon **Client secret**. Confirm the new row is **Enabled**.

### Callback URL

Click **Create a callback URL**. Paste the **Callback URL** you copied from IDmelon and save it.

The value must match the IDmelon field, including `http` or `https` and the path `/api/oidc/sp/callback`. A URL already in the list for another product, such as an identity-verification page or a mobile link, is not this callback. Add the IDmelon one even if other URLs are already there.

Do not click **Add an OIDC Delegate**. IDmelon talks to Nametag directly.

## Finishing IDmelon Configuration as SP

Return to **OIDC Client Settings**.

- Paste the Nametag **Client ID**.
- Paste the API key into **Client secret**.
- Click **Next**.

On **Scopes and claims**:

- **Scopes**: leave `openid` and `profile`. `"openid"` is always included. Do not add `email`.
- **Requested claims**: leave `given_name` and `family_name`. Nametag returns a claim only when it is asked for by name.

Click **Next**.

![Nametag IDmelon 5](/images/vendor/sso/nametag/nametag_16.png)

On **App Attributes Mapping**, keep the two rows and click **Confirm**:

| Claim | IDmelon user field |
| --- | --- |
| `given_name` | `firstName` |
| `family_name` | `lastName` |

![Nametag IDmelon 6](/images/vendor/sso/nametag/nametag_17.png)

You return to **External Identity Providers**. The new row shows **Nametag**, protocol **OIDC**, and status **Ready**.

![Nametag IDmelon 7](/images/vendor/sso/nametag/nametag_18.png)

The user must already exist in IDmelon with the same first and last name.

Open **Authentication > Authentication Profile**. Under **Select Identity Provider**, choose **External**. The panel reports how many external IdPs are set up for sign-in. Click **Save** if the control is enabled.

![Nametag IDmelon 8](/images/vendor/sso/nametag/nametag_19.png)

Users can then sign in through Nametag. After verification, Nametag sends the browser to the callback URL you added.
