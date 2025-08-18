---
title: "SCIM Synchronization with Okta"
description: "Configure SCIM synchronization between Okta and IDmelon for automated user provisioning"
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

This document describes how to configure SCIM synchronization between Okta and IDmelon for automated user provisioning.

## Setup IDmelon For Provisioning

Log in to the IDmelon panel, go to **App Integrations** > **SCIM**, and click **Okta**.

![IDmelon Panel - SCIM Configuration](/images/vendor/scim/okta/panel_01.png)

Enable the connection by clicking the switch button.

![IDmelon Panel - Enable Connection](/images/vendor/scim/okta/panel_02.png)

You can see the **SCIM URL** and **SCIM Token** to use in the Okta panel.

![IDmelon Panel - SCIM Credentials](/images/vendor/scim/okta/panel_03.png)

> The newly generated token will be shown only once. Make sure to copy and save it.

> If you created the first token and want to generate a new token, you can click on the **New SCIM Token** icon.

## Setup Okta For Provisioning

From the Okta admin panel, click on the **Applications** menu, and then click on **Browse App Catalog**.

![Okta - Browse App Catalog](/images/vendor/scim/okta/01.png)

In the **Browse App Integration Catalog**, search for **SCIM**, and select the **SCIM 2.0 Test APP (Header Auth)**.

![Okta - SCIM 2.0 Test APP](/images/vendor/scim/okta/02.png)

Then from the **SCIM 2.0 Test APP (Header Auth)** page, click on **Add Integration**.

![Okta - Add Integration](/images/vendor/scim/okta/03.png)

Click on **General Settings** and in the **Application label** field, enter **SCIM 2.0 - IDmelon**.

![Okta - General Settings](/images/vendor/scim/okta/04.png)

![Okta - Application Label](/images/vendor/scim/okta/05.png)

Go to the next **Sign-On Options** and click the **Done** button.

![Okta - Sign-On Options](/images/vendor/scim/okta/06.png)

Now go to the **Provisioning** tab of the created app, and click on **Configure API Integration**.

![Okta - Configure API Integration](/images/vendor/scim/okta/08.png)

Click **Enable API Integration** and fill in the **Base URL** and **API Token** based on the values received from the IDmelon Panel.

![Okta - API Integration Settings](/images/vendor/scim/okta/09.png)

Click on the **Test API Credentials** button.

![Okta - Test API Credentials](/images/vendor/scim/okta/10.png)

You should see a success message. Now click the **Save** button.

![Okta - Save Configuration](/images/vendor/scim/okta/11.png)

Go to the **Provisioning** tab and select the **To App** settings from the sidebar menu, then click the **Edit** button.

![Okta - To App Settings](/images/vendor/scim/okta/13.png)

Enable the checkboxes for **Create Users**, **Update User Attributes**, and **Deactivate Users**, and then click the **Save** button.

![Okta - Provisioning Settings](/images/vendor/scim/okta/14.png)

### Sync Users of One Group

To sync all users inside a group:

Go to the **Groups** directory and select the group you want to sync.

![Okta - Select Group](/images/vendor/scim/okta/15.png)

Go to the **Applications** tab and click on the **Assign Application** button.

![Okta - Assign Application](/images/vendor/scim/okta/16.png)

From the list, click on the **Assign** button for the **SCIM 2.0 - IDmelon** application.

![Okta - Assign SCIM Application](/images/vendor/scim/okta/17.png)

Click the **Save and Go Back** button and then the **Done** button.

![Okta - Save Assignment](/images/vendor/scim/okta/18.png)

![Okta - Assignment Complete](/images/vendor/scim/okta/19.png)

Now every user or new user in this group will be synced automatically.

### Sync Group

If you want to sync the group itself, go to the **Push Groups** tab of the **SCIM 2.0 - IDmelon** application, click on **Push Groups**, select **Find groups by name**, find your group, and then click on the **Save** button.

![Okta - Push Groups](/images/vendor/scim/okta/20.png)

![Okta - Find Groups by Name](/images/vendor/scim/okta/21.png)

![Okta - Select Group to Push](/images/vendor/scim/okta/22.png)

![Okta - Group Push Settings](/images/vendor/scim/okta/23.png)

![Okta - Save Group Push](/images/vendor/scim/okta/24.png)

![Okta - Group Push Complete](/images/vendor/scim/okta/25.png)
