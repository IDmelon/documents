---
title: "Add a SAML 2.0 IdP"
description: "Configuring an External IdP via SAML 2.0"
lead: ""
date: 2025-02-04T16:45:00+03:30
lastmod: 2025-02-04T16:45:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "identity_providers"
weight: 10
toc: true
---

You can connect IDmelon as a SP to any identity provider supported by SAML.

By default, settings for common identity providers have been added.

If your identity provider is not in that list, you can connect it by creating a custom integration.

## Configure an Identity provider

Follow the steps below to create a custom integration:

Go to the `External Idp` section under `Authentication`.

![alt](/images/vendor/sso/custom_external_idp/external_idp_01.png)

Click on `Integrate with a New Identity Provider`, Then click on `Create a custom configuration`.

![alt](/images/vendor/sso/custom_external_idp/external_idp_02.png)

Fill in the required fields in the `APP Profile` section,

Choose a desired name for the `Name` field.

Then click `Next`.

![alt](/images/vendor/sso/custom_external_idp/external_idp_03.png)

Enter the requested information in the `App Settings` section.

For the `Entity ID` field, enter the value provided by your identity provider.

Match the values of `Single Sign-On Service Binding` and `Single Logout Service Binding` with your identity provider.

Enter the value provided by your identity provider for the `Single Sign-On Service` field.

Match the `Name Id Format` value with your identity provider.

Fill `Name ID Field` field , the valid values are `EMALI` and `AzureID`.

Upload the public certificate corresponding to your identity provider for the `Public Certificate` field.

If you select `Is Assertion Encrypted?`, the `Encrypt Certificate` should be uploaded.

Upload the metadata corresponding to your identity provider for the `Metadata` field.

Click `Next`.

![alt](/images/vendor/sso/custom_external_idp/external_idp_04.png)
![alt](/images/vendor/sso/custom_external_idp/external_idp_05.png)

In the `App Attributes Mapping` section, add the required fields for mapping attributes.

The valid values for the `SP variable name` section are:

- `EMAIL`
- `LASTNAME`
- `FIRSTNAME`
- `USERNAME`
- `GROUPS`

Click `Confirm`.

![alt](/images/vendor/sso/custom_external_idp/external_idp_06.png)

Now, from the `Authentication` menu, click on `External Idp`.

Then, from the list of applications, select the created application and click on `Edit`.

![alt](/images/vendor/sso/custom_external_idp/external_idp_07.png)

In the `General Settings` section, information related to `SP Entity ID`, `SP Assertion Consumer Service URL` and `SP Single Logout URL` is displayed.
You need to enter these values in your service provider.

![alt](/images/vendor/sso/custom_external_idp/external_idp_08.png)
