---
title: "Get Started"
description: "Overview of getting started with IDmelon documentation and key sections."
lead: ""
date: 2023-09-11T11:37:06+03:30
lastmod: 2023-09-11T11:37:06+03:30
draft: false
images: []
menu:
  docs:
    parent: "admin"
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
| Create Your Admin Panel          | [Sign up and set up your IDmelon admin panel to manage users, keys, and settings.](/docs/get_started/admin/create_an_administration_panel/) |
| User Management                  | [Import users from Azure AD, LDAP, or CSV, or add them manually.](/docs/manage_panel/users/) |
| Assign Security Keys             | [Assign smartphones, contactless cards (ID badges), or biometric as a FIDO2 security key to users.](/docs/manage_panel/security_keys/) |
| Provision Passkeys               | [Provision Microsoft and other relying party (RP) passkeys on behalf of users.](/docs/manage_panel/passkeys/) |
| Windows Login (On-Prem)          | [Enable login to on-prem Windows devices using the FIDO Credential Provider (FCP).](/docs/windows_login/local/onpremise_domain_logon/) |
| Password Autofill                | [Use badge tap to autofill credentials on platforms that do not support FIDO2.](/docs/app_logins/legacy_passwords/) |
| Security Key Policies            | [Manage PIN behavior, online/offline usage preferences.](/docs/manage_panel/security_keys/fido2_client_pin_complexity/) |
| Shared account setup             | [Configure shared user accounts for environments where multiple users access the same device.](/docs/manage_panel/users/shared_accounts/) |
| Conditional Access               | [Define access policies based on IP address, key type, time, device, and relying party.](/docs/manage_panel/security_key_policies/) |
| Admin Roles & Delegation         | [Assign admin roles at the workspace or organizational unit (OU) level.](/docs/manage_panel/user_permissions/) |
| Tap-to-action Workflows          | [Enable tap-to-login and tap-to-logout on shared or kiosk workstations using IDmelon workflows.](/docs/workflow_automation/tap_to_action/) |
| SSO Integration (Single Sign-On) | [Configure IDmelon as your organization's SSO solution to allow passwordless access to multiple applications using a single authentication flow.](/docs/app_logins/sso/) |
| Service Provider (SP) Setup      | [Use IDmelon as a SAML Service Provider to authenticate users via your existing Identity Provider (IdP). Ideal for organizations that prefer central IdP control with IDmelon handling authentication workflows.](/docs/manage_panel/external_idp/) |
| Dedicated Deployment Option      | [Deploy IDmelon on a private cloud or on-premises infrastructure based on your organization’s compliance, data residency, or operational requirements. Suitable for enterprises that require a self-hosted or dedicated environment instead of SaaS.](/docs/deployment/deployment_options/) |
| Automatic Software Deployment    | [Deploy IDmelon components (e.g., FIDO Credential Provider, Pairing Tool, Reader Driver) across endpoints using tools like Intune, GPO, or third-party deployment solutions. Ideal for automating installation at scale.](/docs/deployment/exe_apps/) |

</div>
