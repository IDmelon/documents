---
title: "MFA Fallback Sign-in Experience"
description: "Configure IDmelon FCP software MFA fallback authentication when FIDO2 security keys are unavailable, using OTP over SMS, Email, and Call methods."
lead: "Learn how users experience MFA fallback authentication when FIDO2 security keys are not available and how to configure the fallback options."
date: 2025-01-20T15:32:55+03:30
lastmod: 2025-01-20T15:32:55+03:30
draft: false
images: []
menu:
  docs:
    parent: "windows_passwordless_deployment"
weight: 89200
toc: true
---

## Overview

When FIDO2 security keys are unavailable for authentication on Windows 10/11 PCs, users need a secure fallback mechanism.

## Configure Credetial Providers

IDmelon FCP can be configured using the command `idmelonfcp` with the following options:

```bash
idmelonfcp mode -s [MODE]
```

Available modes:

- **DirectFidoAuth**: Login with FIDO security key only
- **MFA**: Login with password and a second authentication method. Use this mode for Azure AD/Hybrid Azure AD joined PCs where FIDO2 is already available through Windows Hello.
- **Hybrid**: Supports both FIDO2 security key and password + MFA. Use this mode for AD domain joined PCs to add FIDO2 security key option while protecting password authentication.

### MFA Method Configuration

In case of MFA, using the command below you can specify the 2nd authenticaiton factor.

```bash
idmelonfcp mfa-method -s [METHOD]
```

Available methods:

- **SMS**: SMS-based OTP delivery
- **Email**: Email-based OTP delivery  
- **Call**: Phone call-based verification
- **All**: All methods available (default)

## Configuration Examples

 IDmelon FCP software provides different authentication modes based on the Windows environment.

- **Azure AD/Hybrid Azure AD joined PCs**: FIDO2 security key authentication is available through Windows Hello. MFA mode provides fallback when security keys are unavailable.
- **AD domain joined PCs**: Hybrid mode is required to add FIDO2 security key option while also protecting password authentication with MFA.

Both scenarios ensure users can always access their systems while maintaining strong security standards.

### Enable MFA Mode (Azure AD/Hybrid Azure AD joined PCs)

For Azure AD or Hybrid Azure AD joined PCs where FIDO2 security key authentication is already available through Windows Hello, use MFA mode for fallback authentication:

```bash
idmelonfcp mode -s MFA
```

### Enable Hybrid Mode (AD domain joined PCs)

For AD domain joined PCs where you need to add FIDO2 security key option while also protecting password authentication with MFA:

```bash
idmelonfcp mode -s Hybrid
```

### Set SMS as Default MFA Method

To set SMS as the default MFA method:

```bash
idmelonfcp mfa-method -s SMS
```

## Example: User Experience with MFA Fallback

After entering their username and password, users will be prompted to enter an MFA code. In this example, the system is configured to automatically send the code via email:

![Windows MFA Email Prompt](/images/vendor/DirectoryServicesIntegration/FCP/mfa-fallback-email-example.gif)

> The user sees a prompt:
> "Please enter the code sent to [user’s email address]"
> They must enter the MFA code received in their email to complete the sign-in process.

You can configure the system to send the MFA code via SMS, Email, or Call, depending on your organization's policy and the selected `mfa-method`.
