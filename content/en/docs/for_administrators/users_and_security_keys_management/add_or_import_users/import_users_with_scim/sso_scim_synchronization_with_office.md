---
title: "SSO SCIM Synchronization with Entra ID"
description: ""
lead: ""
date: 2023-09-20T13:36:09+03:30
lastmod: 2023-09-20T13:36:09+03:30
draft: false
images: []
menu:
  docs:
    parent: "import_users_with_scim"
weight: 31410
toc: true
---

This document is about SSO SCIM synchronization with Azure Active Directory.

## Setup IDmelon For Provisioning

---

Log in to the IDmelon panel, go to **Users > All Users**, click **Import Users**, and select **SCIM Connector**.

![alt](/images/vendor/scim/azure/panel_01.png)

Select `Microsoft Entra ID` as identity provider, then click `Next`.

![alt](/images/vendor/scim/azure/panel_02.png)

Click `Generate New Token`.

![alt](/images/vendor/scim/azure/panel_03.png)

> The newly generated token will be shown only once. Make sure to copy and save it.

Finally click `Active` button.

![alt](/images/vendor/scim/azure/panel_04.png)

Now IDmelon is ready for provisioning.

![alt](/images/vendor/scim/azure/panel_05.png)

## Setup Azure For Provisioning

---

### Step 1 - Login to azure

Login to the admin azure panel from here:  https://portal.azure.com/#home ,
    and click the `Enterprise applications`.

![alt](/images/vendor/scim/azure/01.png)

### Step 2 - Enterprise applications

From the `All appliacations` menu, click to the `New application` .

![alt](/images/vendor/scim/azure/02.png)

### Step 3 - Create your own new application

Click on the `Create your own application`.

![alt](/images/vendor/scim/azure/03.png)

### Step 4 - Configure your own new application

Fill fields of opened from as:

* What's the name of your app?
  * IDmelon
* What are you looking to do with your application?
  * Integrate any other application you don't find in the gallery (Non-gallery)

And then Click `Create` button.

![alt](/images/vendor/scim/azure/04.png)

### Step 5 - Provisioning

In the opened page, click the `Get started` of the `Provision User Accounts` section.

![alt](/images/vendor/scim/azure/05.png)

### Step 6- Provisioning

And then in the next page, click the `Get started` from main again.

![alt](/images/vendor/scim/azure/06.png)

### Step 7 - Update credentials

And then in the next page, fill fields of opened from as:

* Provisioning mode
  * Automatic

And in the `Admin Credentials` section:

* Tenant URL
  * https://skm.idmelon.com/api/scim/v2/

* Secret Token
  * PASTE THIS VALUE FROM IDMELON PANEL

And click `Test connection` to check the connection to `IDmelon` is successful.

Then click `Save`.

![alt](/images/vendor/scim/azure/07.png)

### Step 7 - Update Settings

Based on your situation, change the Scope to `Sync all users and groups`

![alt](/images/vendor/scim/azure/08.png)

### Step 8 - User Attribute mapping

Expand the `Mapping` section and click on `Provision Microsoft Entra ID Users`

![alt](/images/vendor/scim/azure/09.png)

### Step 9 - User attribute mapping - advance options

In the `Attribute Mappings` section, check the `Show advanced options` option,
and then click on the `Edit attribute list for customappsso`:

![alt](/images/vendor/scim/azure/10.png)

Add a new `immutableId` field based as picture and below table and then click the `Save`.

![alt](/images/vendor/scim/azure/11.png)

| Name                                                                 | Type   |
|----------------------------------------------------------------------|--------|
| urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:immutable_id | String |

### Step 10 - User attribute mapping

Back to the `Attribute Mappings` section, and from the default list, apply these two changes and then click the `Save`.

* Change the  `objectId` field by clicking on the `Edit` button.
* Add `immutableId` field by clicking the `Add New Mapping`.

| customappsso Attribute  | Microsoft Entra ID Attribute                                          | Matching precedence |
|-------------------------|-----------------------------------------------------------------------|---------------------|
| objectId                | externalId                                                            |                     |
| immutableId             | urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:immutable_id  |                     |

![alt](/images/vendor/scim/azure/12.png)
![alt](/images/vendor/scim/azure/13.png)
![alt](/images/vendor/scim/azure/14.png)
![alt](/images/vendor/scim/azure/15.png)

### Step 11 - Group Attribute mapping

Expand the `Mapping` section and click on `Provision Microsoft Entra ID Group`

![alt](/images/vendor/scim/azure/16.png)

### Step 12 - Group attribute mapping - advance options

In the `Attribute Mappings` section, check the `Show advanced options` option,
and then click on the `Edit attribute list for customappsso`:

![alt](/images/vendor/scim/azure/17.png)

Add a new `description` field based as picture and below table and then click the `Save`.

![alt](/images/vendor/scim/azure/18.png)

| Name           | Type   |
|----------------|--------|
| description    | String |

### Step 13 - Group attribute mapping

Back to the `Attribute Mappings` section, and from the default list, apply this change and then click the `Save`.

* Add `description` field by clicking the `Add New Mapping`.

| customappsso Attribute | Microsoft Entra ID Attribute  | Matching precedence |
|------------------------|-------------------------------|---------------------|
| description            | description                   |                     |

![alt](/images/vendor/scim/azure/19.png)
![alt](/images/vendor/scim/azure/20.png)
![alt](/images/vendor/scim/azure/21.png)

### Deprovisioning

The rules of deprovisioning are as follows:

#### Sync Only Assigned Users and Groups

If you have set the SCIM sync to be dependent on specific users and groups (`Sync only assigned users and groups`), removing a user on the IDmelon side can be done in the following ways:

* Method 1: Remove the user from the specified group on the Entra ID side.
* Method 2: Disable the user on the Entra ID side.
* Method 3: Delete the user on the Entra ID side.

#### Sync All Users and Groups

If the SCIM sync is set to include all users and groups (`Sync all users and groups`), removing a user on the IDmelon side can be done as follows:

* Method 1: Disable the user on the Entra ID side.
* Method 2: Delete the user on the Entra ID side.
