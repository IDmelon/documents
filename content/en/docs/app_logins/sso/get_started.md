---
title: "Get started with app integrations"
description: "Introduction to IDmelon SSO as an IdP or with an existing IdP to minimize number of log-ins."
lead: ""
date: 2023-09-11T15:28:26+03:30
lastmod: 2023-09-11T15:28:26+03:30
draft: false
images: []
menu:
  docs:
    parent: "app_integrations"
weight: 5
toc: true
---

The IDmelon Passkey Orchestration Platform extends its capabilities beyond FIDO2 authenticators by offering Single Sign-On (SSO) services. This allows enterprises to provide a secure gateway to essential applications for users. Whether deploying a new SSO solution or enhancing an existing one, IDmelon SSO can function as an Identity Provider (IdP) or integrate with an existing enterprise IdP, ensuring seamless authentication using federated protocols.

---

## Supported Federated Authentication Protocols

IDmelon supports industry-standard federated authentication protocols, ensuring compatibility with various enterprise environments:

- **SAML 2.0 (Security Assertion Markup Language)**  
  Used for authenticating users across enterprise applications, cloud services, and SaaS platforms.

- **OIDC (OpenID Connect)**  
  An identity layer built on OAuth 2.0, providing secure authentication for modern web and mobile applications.

---

## Deployment Models

IDmelon’s flexible SSO integration allows for two primary deployment models.

### Integrate with an Existing Identity Provider (aka. IDmelon SSO for SSO)

IDmelon can be deployed in front of an organization’s existing IdP, enhancing security without modifying the existing identity infrastructure. It supports seamless authentication with identity providers, including:

- Microsoft Entra ID (Azure AD)
- Active Directory Federation Services (ADFS)
- Okta
- Ping Identity
- Google Workspace
- Other SAML/OIDC-compatible IdPs

One key advantage of this model is that no additional user passkey provisioning is required on the identity provider. Unlike traditional IdPs that require manual user onboarding or directory synchronization, IDmelon automatically creates a passkey when a user activates an Dmelon-supported FIDO2 authenticator (e.g., smartphone, RFID/NFC badge, biometrics).

### Standalone Identity Provider (IdP Model)

IDmelon can also function as a full-fledged identity provider, directly managing authentication for various enterprise applications without requiring an external IdP. In this mode, organizations can:

- Provide a unified authentication experience, allowing users to log in without entering usernames or passwords by relying solely on FIDO2 passkeys.
- Protect both internal and cloud applications through SAML 2.0 and OIDC protocols.

---

## Enterprise Use Cases

IDmelon’s SSO service offers secure and scalable authentication across different industries and applications. Its key benefits include:

- Workforce and Enterprise Authentication: Employees can access platforms such as Microsoft 365, Google Workspace, Salesforce, and ServiceNow using passkeys instead of passwords.

- Virtual Desktop Environments (VDI): IDmelon ensures passwordless authentication for users accessing Citrix and VMware Horizon.

- Healthcare Security Compliance: Medical professionals can authenticate using RFID/NFC badges and biometrics, ensuring compliance with strict security standards.

- Hybrid and On-Prem Access Control: Organizations can secure intranet portals, VPNs, and legacy applications with federated authentication.

- Enhanced Multi-Factor Authentication (MFA): IDmelon eliminates SMS OTPs and weak MFA methods, replacing them with secure passkey-based authentication for enterprise-wide security.

---
