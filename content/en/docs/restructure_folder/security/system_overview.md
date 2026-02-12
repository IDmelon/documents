---
title: "System Overview"
description: "High-level overview of the IDmelon system architecture and security model."
lead: "This section will provide an overview of the system, the components that make up the system, and the key components for IT managers. It worth mentioning that the system is designed to be intuitive and user-friendly and can be tailored for any business with any size. IDmelon is built on a robust and scalable cloud infrastructure, using the latest security standards and technologies (such as Google HSM) to ensure the highest levels of data protection and privacy. IDmelon offers a wide range of services and integrations, including single sign-on, Azure AD integration, user management, and access control as well as its main service that enables FIDO in many environments; whether it is on-premise or cloud based. It is also designed to be highly scalable and customizable for enterprise organizations, allowing us to easily adapt it to their infrastructure and environments needs."
date: 2022-02-16T18:14:02-08:00
lastmod: 2022-02-16T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "security"
weight: 900000
toc: true
---

## Abstracted System Overview

---

This abstracted system architecture overview outlines the components and services used to build and run IDmelon
Passwordless Orchestration Platform (IPOP) on Google Cloud Platform (GCP). It explains how the components interact with
each other to provide the application’s functionalities. It also provides an overview of how the application is deployed
and configured on the Cloud.

![alt](/images/vendor/Security/Security_5.png)

## System Security

---

IPOP has a comprehensive security strategy that provides confidentiality, integrity, and availability; all designed by
our security experts.

On the layer of data security, we address the latest security measures. Credentials are stored encrypted or in Google
Hardware Security Module (HSM), and keys are securely managed by Google Secret Manager to ensure the latest security
measures are always applied.

Concerning data transfer, we have made sure to implement best practices such as input validations, TLS, Cloudflare
protections, data encryption, CORS, sanitization, and many more.

## Sync with Azure AD

---

To sync groups, users, and devices with Microsoft Azure Active Directory, we safely follow the official Microsoft
GraphAPI and documentation. The following diagram depicts an overview of the sync process:

![alt](/images/vendor/Security/Security_6.png)

Briefly, the permissions and APIs are as follows:

1. **Read all groups:** Permission: GroupMember.Read.All\
   URL: [https://graph.microsoft.com/v1.0/groups](https://graph.microsoft.com/v1.0/groups)
2. **Read all groups members:** Permission: GroupMember.Read.All\
   URL: [https://graph.microsoft.com/v1.0/groups/${GROUP_ID}/members](https://graph.microsoft.com/v1.0/groups/${GROUP_ID}/members)
3. **Read connected devices (Windows devices only):** Permissions: Directory.Read.All, Device.Read.All\
   URL: [https://graph.microsoft.com/v1.0/devices?$filter=operatingSystem](<https://graph.microsoft.com/v1.0/devices?$filter=operatingSystem eq 'Windows'>)
4. **Get temporary access pass:** Permission: UserAuthenticationMethod.ReadWrite.All\
   URL: [https://graph.microsoft.com/v1.0/users/${USER_GUID}/authentication/temporaryAccessPassMethods](https://graph.microsoft.com/v1.0/users/${USER_GUID}/authentication/temporaryAccessPass)

## System Updates

---

IDmelon leverages automation CI/CD and GCP tools to ensure that all components are updated in a coordinated and timely
manner. To ensure that the system remains stable during the upgrade process, we will use backup and rollback strategies
to ensure that any unexpected issues can be quickly addressed. Additionally, our rolling release strategy ensures that
users always receive the latest updates as quickly as possible. Finally, we use monitoring tools to ensure that the
system is performing as expected and that any potential problems are quickly identified and addressed.
