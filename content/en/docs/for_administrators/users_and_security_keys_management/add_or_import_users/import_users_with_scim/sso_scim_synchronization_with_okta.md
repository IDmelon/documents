---
title: "SSO SCIM Synchronization with Okta"
description: ""
lead: ""
date: 2023-09-20T13:36:09+03:30
lastmod: 2023-09-20T13:36:09+03:30
draft: false
images: []
menu:
  docs:
    parent: "import_users_with_scim"
weight: 31420
toc: true
---

This document is about SSO SCIM synchronization with Okta.

## Setup IDmelon For Provisioning
---

Log in to the IDmelon panel, go to **Users > All Users**, click **Import Users**, and select **SCIM Connector**.

![alt](/images/vendor/scim/okta/panel_01.png)

Select `Okta` as identity provider, then click `Next`.

![alt](/images/vendor/scim/okta/panel_02.png)

Click `Generate New Token`.

![alt](/images/vendor/scim/okta/panel_03.png)

> The newly generated token will be shown only once. Make sure to copy and save it.


Finally click `Active` button.

![alt](/images/vendor/scim/okta/panel_04.png)

Now IDmelon is ready for provisioning.

![alt](/images/vendor/scim/okta/panel_05.png)

## Setup Okta For Provisioning
---

From the Okta admin panel, click on the `Applications` menu. And then click on the `Browse App catalog`

![alt](/images/vendor/scim/okta/01.png)

In the `Browse App Integration Catalog`, search for `SCIM`, And select the `SCIM 2.0 Test APP (Header Auth)`

![alt](/images/vendor/scim/okta/02.png)

Then from the `SCIM 2.0 Test APP (Header Auth)` page click on the `Add Intgeration`

![alt](/images/vendor/scim/okta/03.png)

Click on the `General Settings` and in the `Applicataion label` field enter the `SCIM 2.0 - IDmelon`

![alt](/images/vendor/scim/okta/04.png)

![alt](/images/vendor/scim/okta/05.png)

Go the next `Sign-On Options` and click `Done` button.

![alt](/images/vendor/scim/okta/06.png)

Now go to the `Provisioning` tab of created app, and click on the `Configuration API Integration`

![alt](/images/vendor/scim/okta/08.png)

Click `Enable API Integration` and Fill the `Base URL` and `API Token` based on received values on the IDmelon Panel.

![alt](/images/vendor/scim/okta/09.png)

Click on the `Test API Credentials` button.

![alt](/images/vendor/scim/okta/10.png)

You should see successfully message, now click the `Save` button.

![alt](/images/vendor/scim/okta/11.png)

Go to the `Provisioning` tab and select the `To App` settings from the sidebar menu and click the `Edit` button.

![alt](/images/vendor/scim/okta/13.png)

Enable the checkboxes for `Create User`, `Update User Attributes`, `Deactive Users`, and then click `save` button.

![alt](/images/vendor/scim/okta/14.png)

### Sync users of one group

To sync all users inside a group,

Go to the `Group` directory and select the group you want to sync.

![alt](/images/vendor/scim/okta/15.png)

Go to the `Applications` tab and click on the `Assgin application` button.

![alt](/images/vendor/scim/okta/16.png)

From the list, Click on the `Assgin` button for the `SCIM 2.0 IDmelon` application.

![alt](/images/vendor/scim/okta/17.png)

Click the `Save and Go Back` button and then `Done` button.

![alt](/images/vendor/scim/okta/18.png)

![alt](/images/vendor/scim/okta/19.png)

Now every user or new user in this group will be sync automatically,

### Sync group

If you want to sync the group itself, Go to the `Push Groups` tab of `SCIM 2.0 IDmelon` application and click on the `Push Groups` and select the `Find groups by name` and find your group and the click on the `Save` button.

![alt](/images/vendor/scim/okta/20.png)

![alt](/images/vendor/scim/okta/21.png)

![alt](/images/vendor/scim/okta/22.png)

![alt](/images/vendor/scim/okta/23.png)

![alt](/images/vendor/scim/okta/24.png)

![alt](/images/vendor/scim/okta/25.png)
