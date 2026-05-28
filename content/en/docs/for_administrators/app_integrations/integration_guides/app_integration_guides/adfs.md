---
title: "ADFS"
description: ""
lead: ""
date: 2023-09-13T00:55:12+03:30
lastmod: 2023-09-13T00:55:12+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72100
toc: true
---

> **⚠️ Important Notice**
>
> The content in this document is currently under review and will be updated soon. We are actively working on providing you with the most accurate and up-to-date information. Thank you for your patience and understanding.

In this document, you are going to set up `IDmelon` as an external IdP to the `ADFS`.

## Open ADFS Management

---

1. Login to the windows server which ADFS is installed on
2. Open Server Manager
![alt](/images/vendor/sso/adfs_01.png)
3. On top right, click on tools and open AD FS Mangement

## Integrate with IDmelon

---

1. On the IDmelon admin panel, click on App integrations
2. Choose Single Sign-on
![alt](/images/vendor/sso/adfs_02.png)
3. Click on New Application
4. Choose "Create a custom configuration"
5. In the app setting, upload ADFS metadata and confirm

## Add Identity Provider

---

1. On the right panel, click on Add Claims Provider Trust
2. Choose "Import data about the claims provider from a file" for select data source and upload idmelon metadata
![alt](/images/vendor/sso/adfs_03.png)
3. Choose your name of choice for display name
4. Click next and confirm creation
