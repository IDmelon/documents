---
title: "Okta"
description: "Integrate Okta with IDmelon"
lead: ""
date: 2025-02-17T23:15:00+03:30
lastmod: 2025-02-17T23:15:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "identity_providers"
weight: 40
toc: true
---

In this document, you are going to set up `Okta` as an external IdP to IDmelon.

## Initialize IDmelon Configuration as SP

---

Log in to the IDmelon panel, navigate to the **Authentication** section, and click **Integrate with a New Identity Provider**:

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_01.png)

Then select **Okta** as the identity provider:

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_02.png)

You will need the values provided in the newly opened window to set up **Okta** as an external IDP:

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_03.png)

## Configuring Okta as external IDP

---

In order to set up the connection, you will need to log into your `Okta administration panel`.

To login to the `Okta administration panel`, click the `Admin` button on the top right corner of OKTA user panel page.

![alt](/images/vendor/sso/okta_external_idp/okta_dashboard_01.png)

### Add Service Provider

---

In the `Okta administration panel`, from the side menu, navigate to the `Applications` menu.

Then from the `Applications` menu, select the `Applications` sub-menu.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_04.png)

Then click on `Create App Integration` to have a custom configuration.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_05.png)

Now select `SAML 2.0` and click `Next`.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_06.png)

On the first page of the configuration, select a name for your Application. Optionally, you can upload an image as an Icon and add a description.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_07.png)

On the following page, you should fill in the following fields:

- **Single sign-on URL:** with the value IDmelon provided in the panel.
- **Audience URL (SP Entity ID):** should be filled with the link you have in the IDmelon panel.
- **Name ID Format:** should be `EmailAddress`
- **Application username:** should be `Email`

Finally, scroll down and click on `Next`.  On the final page, click on `Finish` to create the configuration.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_08.png)

### Finalizing IDmelon Configuration as SP

---

After creating the configuration, navigate to the `Applications` menu and the `Applications` sub-menu once more and select your app.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_09a.png)

Click on the `Sign On` sub-menu.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_09b.png)

you need the following fields for IDmelon, so scroll down and click on `More Detail` to see them.
Copy `Sign on URL`, `Sign out URL`, and `Issuer` to the IDmelon panel, and download the Signing Certificate and upload it to IDmelon. Then click on confirm.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_09c.png)
![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_10.png)

### Assigning Users and Groups

---

Now click on the `Assignments` sub-menu and from People or Groups section, click the `assign` button and assign users or groups that are able to log in to IDmelon.

![Okta IDmelon 1](/images/vendor/sso/okta_external_idp/okta_11.png)

Now both applications are connected. You can assign a group of users or all users of Okta to log in to IDmelon.
