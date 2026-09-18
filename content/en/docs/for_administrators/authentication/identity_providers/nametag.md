---
title: "NameTag"
description: "Integrate NameTag with IDmelon over OpenID Connect"
lead: ""
date: 2026-09-18T00:00:00+00:00
lastmod: 2026-09-18T00:00:00+00:00
draft: false
images: []
menu:
  docs:
    parent: "identity_providers"
weight: 30
toc: true
---

In this document you are going to set up `NameTag` as an external IdP to IDmelon. IDmelon is the relying party. Use the **NameTag** tile marked **OIDC**. Do not use a SAML tile, and do not use **Create a custom configuration**.

## Initialize IDmelon Configuration as SP

Log in to the IDmelon panel. In the left menu open **Authentication**, then **External Identity Providers**.

Click **+ New Identity Provider**.

![NameTag IDmelon 1](/images/vendor/sso/nametag/nametag_11.png)

The next screen is **Select an App**. Under **Featured Applications**, click the **NameTag** card. Its tag is **OIDC**. The **Entra ID** card tagged **SAML 2.0** is a different integration.

![NameTag IDmelon 2](/images/vendor/sso/nametag/nametag_12.png)

The wizard title is **NameTag Integration**. The steps are **App Profile**, **OIDC Client Settings**, **Scopes**, and **App Attributes Mapping**.

On **App Profile**, **Application Name** is already `NameTag`. Leave it and click **Next**.

![NameTag IDmelon 3](/images/vendor/sso/nametag/nametag_14.png)

**OIDC Client Settings** opens. Copy **Callback URL** with the copy button. You will add that exact value in the NameTag console. The host depends on your environment. The path is `/api/sso/oidc/sp/callback`. Leave this window open.

**Discovery URL** is already `https://nametag.co/.well-known/openid-configuration`. Click the fetch button at the end of that field. IDmelon fills:

- **Authorization endpoint**: `https://nametag.co/oauth2/authorize`
- **Token endpoint**: `https://nametag.co/oauth2/token`
- **JWKS URI**: `https://nametag.co/.well-known/jwks`

**UserInfo endpoint** can stay empty. NameTag does not require it for this integration. Do not type the endpoints by hand.

**Client ID** and **Client secret** stay empty until you copy them from NameTag.

![NameTag IDmelon 4](/images/vendor/sso/nametag/nametag_15.png)

## Configuring NameTag

Sign in at [console.nametag.co](https://console.nametag.co). Open the environment you will use. A Sandbox environment shows a banner that the brand is not verified and that requests are not billed. Production does not show that banner.

In the left sidebar, under **Environment settings**, open **OAuth**.

![NameTag Window 1](/images/vendor/sso/nametag/nametag_01.png)

On this page set three things. Do not use **Organization settings > API keys**. That key is not the OAuth client secret.

### Client ID

Copy **Client ID** with the **Copy** button. This is the public part of the API credentials. Paste it into IDmelon **Client ID**.

### Client Secret

If the table already has a key and its **Enabled** switch is on, you can reuse it only if you still have the full secret. The table shows a prefix, not the secret.

Otherwise click **Create new API key**. Keep the default name, create the key, and copy the full value before you leave the dialog. NameTag shows it only once. That value is the IDmelon **Client secret**. Confirm the new row is **Enabled**.

### Callback URL

Click **Create a callback URL**. Paste the **Callback URL** you copied from IDmelon and save it.

The value must match the IDmelon field, including `http` or `https` and the path `/api/sso/oidc/sp/callback`. A URL already in the list for another product, such as an identity-verification page or a mobile link, is not this callback. Add the IDmelon one even if other URLs are already there.

![NameTag Window 2](/images/vendor/sso/nametag/nametag_05.png)

Do not click **Add an OIDC Delegate**. IDmelon talks to NameTag directly.

## Finishing IDmelon Configuration as SP

Return to **OIDC Client Settings**.

- Paste the NameTag **Client ID**.
- Paste the API key into **Client secret**.
- Click **Next**.

On **Scopes**, leave `openid`, `profile`, and `email`. The list is locked. Click **Next**.

On **App Attributes Mapping**, keep the two rows and click **Confirm**:

| IDP variable name | SP variable name |
| --- | --- |
| `given_name` | `firstName` |
| `family_name` | `lastName` |

The user must already exist in IDmelon with the same first and last name.

Open **Authentication > Authentication Profile** and select **External** in **Identity Provider**.

Users can then sign in through NameTag. After verification, NameTag sends the browser to the callback URL you added.
