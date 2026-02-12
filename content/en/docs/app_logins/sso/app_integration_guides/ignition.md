---
title: "Ignition"
description: "How to integrate IDmelon with Ignition for single sign-on."
lead: ""
date: 2025-02-04T00:10:34+03:30
lastmod: 2025-02-04T00:10:34+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72100
toc: true
---

This document describes how to set up `IDmelon` as an external Identity Provider (IdP) for `Ignition`.

## Accessing the Ignition Configuration Panel

---

To configure the connection, log in to your Ignition configuration panel.  Click "Config" on the left side of the Ignition panel page.
![alt](/images/vendor/sso/ignition/ignition_01.png)

## Configuring the Identity Provider in Ignition

---
Navigate to "Identity Provider" within the "Security" section. Click "Create new Identity Provider" to begin creating an IdP for `Ignition`.
![alt](/images/vendor/sso/ignition/ignition_02.png)

On the first page, select "Security Assertion Markup Language 2.0" to integrate `IDmelon` with `Ignition` as an Identity Provider.
![alt](/images/vendor/sso/ignition/ignition_03.png)

After selecting the IdP type, you'll be on the main integration page.  First, provide a `name` and `description` for your configuration.
![alt](/images/vendor/sso/ignition/ignition_04.png)

> Then, scroll down to view the Service Provider (SP) configurations.

![alt](/images/vendor/sso/ignition/ignition_05.png)

## Configuring the Service Provider in IDmelon

---
Now, access the IDmelon panel and create a new SP configuration for Ignition: \
"Single Sign-on" → "App Management" → "New Application"\
Select "Ignition" to create a new configuration based on the default Ignition configuration.
![alt](/images/vendor/sso/ignition/ignition_06.png)
![alt](/images/vendor/sso/ignition/ignition_07.png)

## Connecting Ignition and IDmelon

---
You now have an IdP configuration in the `IDmelon` panel and an SP configuration in the `Ignition` panel.

- Copy the SP **Entity ID** and **Assertion Consumer Service (ACS) URL** from the ‍‍`Ignition` panel and paste them into the corresponding fields in the `IDmelon` form.
![alt](/images/vendor/sso/ignition/ignition_08.png)

- Scroll down to the "Provider Configuration" section in the IDmelon panel.  Set the IdP Entity ID and Single Sign-On (SSO) Service URL using the links from the IDmelon panel.
![alt](/images/vendor/sso/ignition/ignition_09.png)
- For the default IDmelon configuration, uncheck "Validate Response Signatures."
![alt](/images/vendor/sso/ignition/ignition_10.png)
- Click "Confirm" in the `IDmelon` panel and "Save" in the `Ignition` panel.

## Mapping User Attributes

---
After saving in Ignition, go to "More" → "User Attribute Mapping" to map the login parameters with IDmelon.
![alt](/images/vendor/sso/ignition/ignition_11.png)
Configure the following mappings:

- ID:
  - Type: `direct`
  - Source: `Authentication Response`
  - Path: `/saml2p:Response/saml2:Assertion/saml2:Subject/saml2:NameID/text()`
- Username:
  - Type: `direct`
  - Source: `Authentication Response`
  - Path: `/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name="username"]/saml2:AttributeValue/text()`
- First Name:
  - Type: `direct`
  - Source: `Authentication Response`
  - Path: `/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name="firstname"]/saml2:AttributeValue/text()`
- Last Name:
  - Type: `direct`
  - Source: `Authentication Response`
  - Path: `/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name="lastname"]/saml2:AttributeValue/text()`
- Email:
  - Type: `direct`
  - Source: `Authentication Response`
  - Path: `/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name="email"]/saml2:AttributeValue/text()`
- Roles:
  - Type: `direct`
  - Source: `Authentication Response`
  - Path: `/saml2p:Response/saml2:Assertion/saml2:AttributeStatement/saml2:Attribute[@Name="roles"]/saml2:AttributeValue/text()`

Finally, click "Save."
![alt](/images/vendor/sso/ignition/ignition_12.png)
![alt](/images/vendor/sso/ignition/ignition_13.png)

## Testing the Configuration

---
Test the configuration to ensure everything is working correctly. Go to "More" → "Test Login".

You should be redirected to the IDmelon login page.  Enter your account information. After successful authentication in IDmelon, you should see an XML data response.  Review the other tabs to validate your configuration.
![alt](/images/vendor/sso/ignition/ignition_14.png)

## Managing User Access

---
If the data is correct, the integration is complete. Manage user access by creating internal rules within `Ignition`. The default administration rule is named “Administrator”.  Users in the “Administrator” group in `IDmelon` will have administrator access in `Ignition`.  Manage group names and access as needed.
![alt](/images/vendor/sso/ignition/ignition_15.png)
![alt](/images/vendor/sso/ignition/ignition_16.png)

## Setting IDmelon as the Default Login

---
To change the default login to `IDmelon`, go to "General" configuration in the "Security" section and select your `IDmelon` configuration as the default login role.
![alt](/images/vendor/sso/ignition/ignition_16.png)
