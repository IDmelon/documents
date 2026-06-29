---
title: "SCIM Synchronization with Microsoft Entra ID"
description: "Configure SCIM synchronization between Microsoft Entra ID and IDmelon for automated user provisioning"
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

This document describes how to configure SCIM synchronization between Microsoft Entra ID and IDmelon for automated user provisioning.

## Setup IDmelon For Provisioning

Log in to the IDmelon panel, go to **App Integrations** > **SCIM**, and click **Microsoft Entra ID**.

![IDmelon Panel - SCIM Configuration](/images/vendor/scim/azure/panel_01.png)

Enable the connection by clicking the switch button.

![IDmelon Panel - Enable Connection](/images/vendor/scim/azure/panel_02.png)

You can see the **SCIM URL** and **SCIM Token** to use in the Microsoft Entra ID panel.

![IDmelon Panel - SCIM Credentials](/images/vendor/scim/azure/panel_03.png)

> The newly generated token will be shown only once. Make sure to copy and save it.
> If you created the first token and want to generate a new token, you can click on the **New SCIM Token** icon.

## Setup Microsoft Entra ID For Provisioning

### Step 1 - Login to Microsoft Entra ID

Log in to the Microsoft Entra ID admin panel at: https://portal.azure.com/#home, and click **Enterprise applications**.

![Microsoft Entra ID - Enterprise Applications](/images/vendor/scim/azure/01.png)

### Step 2 - Enterprise applications

From the **All applications** menu, click **New application**.

![Microsoft Entra ID - New Application](/images/vendor/scim/azure/02.png)

### Step 3 - Create your own new application

Click on **Create your own application**.

![Microsoft Entra ID - Create Own Application](/images/vendor/scim/azure/03.png)

### Step 4 - Configure your own new application

Fill in the fields of the opened form as follows:

* **What's the name of your app?**
  * IDmelon
* **What are you looking to do with your application?**
  * Integrate any other application you don't find in the gallery (Non-gallery)

Then click the **Create** button.

![Microsoft Entra ID - Application Configuration](/images/vendor/scim/azure/04.png)

### Step 5 - Provisioning

On the opened page, click **Get started** in the **Provision User Accounts** section.

![Microsoft Entra ID - Provision User Accounts](/images/vendor/scim/azure/05.png)

### Step 6 - Provisioning

On the next page, click **Get started** from the main section again.

![Microsoft Entra ID - Provisioning Setup](/images/vendor/scim/azure/06.png)

### Step 7 - Update credentials

On the next page, fill in the fields of the opened form as follows:

* **Provisioning mode**
  * Automatic

In the **Admin Credentials** section:

* **Tenant URL**
  * https://skm.idmelon.com/api/scim/v2/

* **Secret Token**
  * PASTE THIS VALUE FROM IDMELON PANEL

Click **Test connection** to verify the connection to IDmelon is successful.

Then click **Save**.

![Microsoft Entra ID - Admin Credentials](/images/vendor/scim/azure/07.png)

### Step 8 - Update Settings

Based on your requirements, change the Scope to **Sync all users and groups**.

![Microsoft Entra ID - Sync Settings](/images/vendor/scim/azure/08.png)

### Step 9 - User Attribute mapping

Expand the **Mapping** section and click on **Provision Microsoft Entra ID Users**.

![Microsoft Entra ID - User Provisioning](/images/vendor/scim/azure/09.png)

### Step 10 - User attribute mapping - Advanced options

In the **Attribute Mappings** section, check the **Show advanced options** option, and then click on **Edit attribute list for customappsso**:

![Microsoft Entra ID - Advanced Options](/images/vendor/scim/azure/10.png)

Add a new **immutableId** field as shown in the picture and the table below, then click **Save**.

![Microsoft Entra ID - Add immutableId Field](/images/vendor/scim/azure/11.png)

| Name                                                                 | Type   |
|----------------------------------------------------------------------|--------|
| urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:immutable_id | String |

### Step 11 - User attribute mapping

Return to the **Attribute Mappings** section, and from the default list, apply these two changes and then click **Save**.

* Change the **objectId** field by clicking the **Edit** button.
* Add the **immutableId** field by clicking **Add New Mapping**.

| customappsso Attribute  | Microsoft Entra ID Attribute                                          | Matching precedence |
|-------------------------|-----------------------------------------------------------------------|---------------------|
| objectId                | externalId                                                            |                     |
| immutableId             | urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:immutable_id  |                     |

![Microsoft Entra ID - User Attribute Mapping](/images/vendor/scim/azure/12.png)
![Microsoft Entra ID - Edit objectId](/images/vendor/scim/azure/13.png)
![Microsoft Entra ID - Add immutableId Mapping](/images/vendor/scim/azure/14.png)
![Microsoft Entra ID - Save User Mapping](/images/vendor/scim/azure/15.png)

### Step 12 - Group Attribute mapping

Expand the **Mapping** section and click on **Provision Microsoft Entra ID Group**.

![Microsoft Entra ID - Group Provisioning](/images/vendor/scim/azure/16.png)

### Step 13 - Group attribute mapping - Advanced options

In the **Attribute Mappings** section, check the **Show advanced options** option, and then click on **Edit attribute list for customappsso**:

![Microsoft Entra ID - Group Advanced Options](/images/vendor/scim/azure/17.png)

Add a new **description** field as shown in the picture and the table below, then click **Save**.

![Microsoft Entra ID - Add Description Field](/images/vendor/scim/azure/18.png)

| Name           | Type   |
|----------------|--------|
| description    | String |

### Step 14 - Group attribute mapping

Return to the **Attribute Mappings** section, and from the default list, apply this change and then click **Save**.

* Add the **description** field by clicking **Add New Mapping**.

| customappsso Attribute | Microsoft Entra ID Attribute  | Matching precedence |
|------------------------|-------------------------------|---------------------|
| description            | description                   |                     |

![Microsoft Entra ID - Group Attribute Mapping](/images/vendor/scim/azure/19.png)
![Microsoft Entra ID - Add Description Mapping](/images/vendor/scim/azure/20.png)
![Microsoft Entra ID - Save Group Mapping](/images/vendor/scim/azure/21.png)

## Adding Custom Attributes

To configure custom attributes for SCIM provisioning in Microsoft Entra ID, follow these steps:

### Step 1 - User attribute mapping

Return to the **Attribute Mappings** section, check the **Show advanced options** option, and then click on **Edit attribute list for customappsso**:

![Microsoft Entra ID - Custom Attributes](/images/vendor/scim/azure/22.png)

### Step 2 - Edit attribute list

Add new custom fields as shown in the picture and the table below, then click **Save**.

![Microsoft Entra ID - Add Custom Fields](/images/vendor/scim/azure/23.png)

> You can map up to five custom attributes using the following target fields.

| Name                                                                 | Type   |
|----------------------------------------------------------------------|--------|
| urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:extraAttribute1 | String |
| urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:extraAttribute2 | String |
| urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:extraAttribute3 | String |
| urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:extraAttribute4 | String |
| urn:ietf:params:scim:schemas:extension:IDmelon:2.0:User:extraAttribute5 | String |

### Step 3 - User attribute mapping

Return to the **Attribute Mappings** section once more and click **Add New Mapping**.

![Microsoft Entra ID - Add New Custom Mapping](/images/vendor/scim/azure/24.png)

### Step 4 - Edit attribute

Select the desired Microsoft Entra ID attribute to map in the **Source attribute** field, then choose the corresponding custom IDmelon attribute for the **Target attribute** field. Once both are selected, click **OK**.

![Microsoft Entra ID - Edit Custom Attribute](/images/vendor/scim/azure/25.png)

Finally, **Save** your changes.

## Assigning Administrator Roles via SCIM

IDmelon supports assigning predefined workspace administrator roles to users and groups through SCIM provisioning. This allows Microsoft Entra ID to automatically grant or revoke admin access in IDmelon based on role assignments.

> Role assignments made via SCIM are independent of manual role assignments made in the IDmelon panel. SCIM only manages SCIM-assigned roles and does not affect panel-assigned permissions.

### Available Roles

The following predefined roles can be assigned via SCIM:

| Role Value *(use exactly as shown — case-sensitive)* | Display Name                     | Description |
|------------------------------------------------------|----------------------------------|-------------|
| `GlobalAdministrator`                                | Global Administrator             | Full access to all workspace settings, users, devices, and configurations. Equivalent to workspace owner-level access for day-to-day administration. |
| `AuthenticationAdministrator`                        | Authentication Administrator     | Can manage users' security keys and passkeys, view authentication activities, and configure authentication policies. Cannot access billing or global workspace settings. |
| `BillingAdministrator`                               | Billing Administrator            | Can view and manage billing information, subscription plans, and payment details for the workspace. |
| `ConditionalAccessAdministrator`                     | Conditional Access Administrator | Can configure conditional access policies, manage organization units, and control access rules within the workspace. |

### Step 1 - Define App Roles in Azure

1. Go to **Azure Portal** → **App Registrations** → select your IDmelon application → **App roles**
2. Click **Create app role** for each role listed above and fill in:
   * **Display name**: e.g. `Global Administrator`
   * **Allowed member types**: `Users/Groups`
   * **Value**: e.g. `GlobalAdministrator` *(must match exactly — case-sensitive)*
   * **Enable this app role**: checked
3. Repeat for all four roles.

> Azure does not allow editing the **Value** field after creation. To fix a value, you must disable the role, delete it, and recreate it.

### Step 2 - Configure Attribute Mapping

1. Go to **Enterprise Application** → **Provisioning** → **Edit Attribute Mappings** → **Provision Microsoft Entra ID Users**
2. Find the attribute `roles[primary eq "True"].value` and click **Edit**
3. Set the following and click **OK**, then **Save**:

| Field            | Value                                          |
|------------------|------------------------------------------------|
| Mapping type     | Expression                                     |
| Expression       | `SingleAppRoleAssignment([appRoleAssignments])` |
| Target attribute | `roles[primary eq "True"].value`               |

> **Note:** This mapping supports one role per user at a time. For assigning multiple roles to a single user, use group-based role assignment (see below).

### Step 3 - Assign a Role to a User or Group

1. Go to **Enterprise Application** → **Users and groups** → **Add user/group**
2. Select the user or group
3. In **Select a role**, choose one of the four IDmelon roles
4. Click **Assign**

On the next provisioning cycle (or via **Provision on demand**), IDmelon will automatically grant the selected admin role to the user.

### Revoking a Role

To remove an admin role from a user without removing the user:

1. Go to **Enterprise Application** → **Users and groups**
2. Select the user → **Edit assignment**
3. Change the role to **Default Access** (no role)
4. Trigger a sync via **Provision on demand**

IDmelon will automatically revoke the SCIM-assigned role.

> Deactivating or removing a user from the Enterprise Application will also clear their SCIM-assigned roles in IDmelon.

### Group-Based Role Assignment (Multiple Roles per User)

To assign multiple roles to the same user, use group-based provisioning:

1. Create a dedicated group in Azure for each role, for example:
   * `IDMelon-GlobalAdministrators`
   * `IDMelon-BillingAdministrators`
2. Go to **Enterprise Application** → **Users and groups** → assign the corresponding App Role to each group
3. Add users to the appropriate groups

Each user will receive all roles from the groups they belong to.

## Deprovisioning

The rules for deprovisioning are as follows:

### Sync Only Assigned Users and Groups

If you have set the SCIM sync to be dependent on specific users and groups (**Sync only assigned users and groups**), removing a user on the IDmelon side can be done in the following ways:

* **Method 1**: Remove the user from the specified group on the Microsoft Entra ID side.
* **Method 2**: Disable the user on the Microsoft Entra ID side.
* **Method 3**: Delete the user on the Microsoft Entra ID side.

### Sync All Users and Groups

If the SCIM sync is set to include all users and groups (**Sync all users and groups**), removing a user on the IDmelon side can be done as follows:

* **Method 1**: Disable the user on the Microsoft Entra ID side.
* **Method 2**: Delete the user on the Microsoft Entra ID side.
