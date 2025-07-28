---
title: "Get Started"
description: ""
lead: ""
date: 2023-09-11T11:37:06+03:30
lastmod: 2023-09-11T11:37:06+03:30
draft: false
images: []
menu:
  docs:
    parent: "for_administrators"
weight: 10000
toc: true
---

IDmelon enables organizations to go fully passwordless using smartphones, contactless cards, and biometrics as FIDO2 security keys — without the need to install complex infrastructure. Whether you're deploying IDmelon in a cloud, hybrid, or on-premises environment, this guide walks you through the essential steps to get up and running quickly.

Follow the configuration tasks below based on your use case. You don’t need to complete every section — just the ones relevant to your authentication environment and goals.

## Configuration Tasks Overview

Follow this sequence of configuration tasks to start using IDmelon:

<div class="configuration-tasks">

| **Task**                         | **Description** |
|----------------------------------|-----------------|
| Create Your Admin Panel          | [Sign up and set up your IDmelon admin panel to manage users, keys, and settings.](/docs/for_administrators/create_an_administration_panel/) |
| User Management                  | [Import users from Azure AD, LDAP, or CSV, or add them manually.](/docs/for_administrators/users_and_security_keys_management/add_or_import_users/) |
| Assign Security Keys             | [Assign smartphones, contactless cards (ID badges), or biometric as a FIDO2 security key to users.](/docs/for_administrators/users_and_security_keys_management/assign_security_keys/) |
| Provision Passkeys               | [Provision Microsoft and other relying party (RP) passkeys on behalf of users.](/docs/for_administrators/users_and_security_keys_management/manage_passkeys_and_credentials/) |
| Windows Login (On-Prem)          | [Enable login to on-prem Windows devices using the FIDO Credential Provider (FCP).](/docs/for_administrators/windows_passwordless_deployment/onpremise_domain_logon/) |
| Password Autofill                | [Use badge tap to autofill credentials on platforms that do not support FIDO2.](/docs/for_administrators/passwords_for_legacy_authentication/) |
| Security Key Policies            | [Manage PIN behavior, online/offline usage preferences.](/docs/for_administrators/users_and_security_keys_management/manage_additional_security_key_features/) |
| Shared account setup             | [Configure shared user accounts for environments where multiple users access the same device.](/docs/for_administrators/users_and_security_keys_management/configure_a_shared_user_account/) |
| Conditional Access               | [Define access policies based on IP address, key type, time, device, and relying party.](/docs/for_administrators/conditional_access_for_security_keys/) |
| Admin Roles & Delegation         | [Assign admin roles at the workspace or organizational unit (OU) level.](/docs/for_administrators/organization_unit/) |
| Tap-to-action Workflows          | [Enable tap-to-login and tap-to-logout on shared or kiosk workstations using IDmelon workflows.](/docs/for_administrators/workflow_automation/) |
| SSO Integration (Single Sign-On) | [Configure IDmelon as your organization's SSO solution to allow passwordless access to multiple applications using a single authentication flow.](/docs/for_administrators/app_integrations/) |
| Service Provider (SP) Setup      | [Use IDmelon as a SAML Service Provider to authenticate users via your existing Identity Provider (IdP). Ideal for organizations that prefer central IdP control with IDmelon handling authentication workflows.](/docs/for_administrators/authentication/identity_providers/) |
| Dedicated Deployment Option      | [Deploy IDmelon on a private cloud or on-premises infrastructure based on your organization’s compliance, data residency, or operational requirements. Suitable for enterprises that require a self-hosted or dedicated environment instead of SaaS.](/docs/for_administrators/dedicated-deployment/deployment_options/) |
| Automatic Software Deployment    | [Deploy IDmelon components (e.g., FIDO Credential Provider, Pairing Tool, Reader Driver) across endpoints using tools like Intune, GPO, or third-party deployment solutions. Ideal for automating installation at scale.](/docs/for_administrators/automatic_software_deployment/) |

</div>
