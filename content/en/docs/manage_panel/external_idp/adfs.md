---
title: "Active Directory Federation Service (AD-FS)"
description: "Integrate Active Directory Federation Service (AD-FS) with IDmelon"
lead: ""
date: 2025-02-05T09:15:00+03:30
lastmod: 2025-02-05T09:15:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "identity_providers"
weight: 30
toc: true
---

In this document you are going to set up `AD-FS` as an external IdP to the IDmelon.

## Initialize IDmelon Configuration as SP

Log in to the IDmelon panel, navigate to the **Authentication** section, and click **Integrate with a New Identity Provider**:

![AD-FS IDmelon 1](/images/vendor/sso/adfs/adfs_15.png)

Then select **ADFS** as identity provider:

![AD-FS IDmelon 2](/images/vendor/sso/adfs/adfs_16.png)

You will need the values provided in the newly opened window to set up **ADFS** as an external IDP:

![AD-FS IDmelon 3](/images/vendor/sso/adfs/adfs_17.png)

## Configuring AD-FS as external IDP

Navigate to Server Manager > Tools > AD FS Management > right click on `Relying Party Trust` > Add Relying Party Trust

![AD-FS Window 1](/images/vendor/sso/adfs/adfs_01.png)

- **Select the** Claims Aware **option**

- **Click on** start.

![AD-FS SetUp Window 1](/images/vendor/sso/adfs/adfs_02.png)

- **Select the** Enter data about the relying party manually **option**

- **Click on** Next.

![AD-FS SetUp Window 2](/images/vendor/sso/adfs/adfs_03.png)

- **Select the** Enter data about the relying party manually **option**

- **Click on** Next.

![AD-FS SetUp Window 3](/images/vendor/sso/adfs/adfs_04.png)

- **Provide a** Display name

- **Add a** Description

- **Click on** Next.

![AD-FS SetUp Window 4](/images/vendor/sso/adfs/adfs_05.png)

- **Click on** next _(No Changes)_.

![AD-FS SetUp Window 5](/images/vendor/sso/adfs/adfs_06.png)

- **Select the** Enable support for the SAML 2.0 WebSSO protocol **option**

- **Provide** Relying party service URL **from IDmelon panel**

- **Click on** Next.

![AD-FS SetUp Window 6](/images/vendor/sso/adfs/adfs_07.png)

- **Provide** Relying party trust identifier **from IDmelon panel**

- **Click on** Add.

- **Click on** Next.

![AD-FS SetUp Window 7](/images/vendor/sso/adfs/adfs_08.png)

- **Select** Permit everyone **option**

- **Click on** Next.

![AD-FS SetUp Window 8](/images/vendor/sso/adfs/adfs_09.png)

- **Click on** Next _(No Changes)_.

![AD-FS SetUp Window 9](/images/vendor/sso/adfs/adfs_10.png)

- **Click on** Close _(No Changes)_.

## Configuring Claims (Attribute Mappings)

![AD-FS Window 2](/images/vendor/sso/adfs/adfs_11.png)

- Select your newly created federation configuration, then choose `Edit Claim Issuance Policy` from the right-hand menu.

![AD-FS Claims Window 1](/images/vendor/sso/adfs/adfs_12.png)

- **Click on** Add Rule.

![AD-FS Claims Window 2](/images/vendor/sso/adfs/adfs_13.png)

- **Select** Send LDAP Attributes as Claims **option**

- **Click on** Next

![AD-FS Claims Window 3](/images/vendor/sso/adfs/adfs_14.png)

- **Provide a** name

- **Select** Active directory **option**

- **Select** Claims **options as follows:**
  - **User Principle Name =>** `Name ID`
  - **E-MailAddress =>** `email`
  - **Given Name =>** `firstName`
  - **Surname =>** `lastName`
  - **User Principle Name =>** `username`

- **Click on** Finish

## Finishing IDmelon Configuration as SP

On the AD FS server, open PowerShell and run the following command to retrieve the server name:\
`(Get-AdfsProperties).Hostname`

Return to the **IDmelon Authentication Panel** and complete the required fields as shown in the images.

1. AD-FS certificate
2. https://<ADFS_SERVER_NAME>/adfs/ls/
3. http://<ADFS_SERVER_NAME>/adfs/services/trust

![AD-FS IDmelon 4](/images/vendor/sso/adfs/adfs_18.png)
