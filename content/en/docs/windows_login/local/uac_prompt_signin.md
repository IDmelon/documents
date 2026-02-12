---
title: "UAC Prompt Sign-in Experience with FIDO2 Security Key"
description: "Use your security key when Windows prompts for elevation (UAC)."
lead: ""
date: 2025-01-15T15:35:46+03:30
lastmod: 2025-01-15T15:35:46+03:30
draft: false
images: []
menu:
  docs:
    parent: "windows_passwordless_deployment"
weight: 89100
toc: true
---

User Account Control (UAC) is a key security feature in Windows that prevents unauthorized changes to the system by requiring elevated privileges for certain actions. It ensures that critical administrative tasks or system changes are performed intentionally by verified users. The "Run as" feature further allows programs to be executed with elevated rights, enabling seamless workflows for administrators.

Incorporating FIDO2 security keys into UAC sign-in and "Run as" experiences enhances security and user convenience. These keys eliminate the need for passwords, making it harder for attackers to exploit shared credentials or phishing attempts. IDmelon’s FIDO2-enabled UAC experience ensures a secure and streamlined process for users performing administrative actions.

---

## Requirements

To use FIDO2 security keys for UAC sign-in, ensure the following prerequisites are met:

1. **IDmelon FIDO2 Security Key (NFC/RFID Employee Badge)**:
   - Users must have an enrolled security key.
2. **IDmelon FCP (Credential Provider)**:
   - Install the credential provider on the host system to enable secure authentication.
   - [Download IDmelon FCP](https://idmelon.com/docs/downloads)

---

## User Experience

When a UAC prompt is triggered (e.g., using "Run as" or accessing remote desktops via RDP), users experience the following flow:

![alt](/images/vendor/DirectoryServicesIntegration/FCP/uac-signin.png)

1. **IDmelon Credential Provider**:
   - The UAC dialog displays IDmelon FCP as an available authentication option.
   - Users select the IDmelon FCP to proceed.

2. **Tapping the Security Key**:
   - The system prompts the user to **tap their badge** on the reader or insert it into a USB port.
   - Upon tapping, the system requests the **security key PIN**.

3. **First-Time AD Credential Prompt**:
   - During the first authentication, the system requests the user's **Active Directory credentials**.
   - For subsequent authentications, only the security key PIN is required.

4. **Successful Login**:
   - Upon successful authentication, users gain access to the requested application.
   - This workflow supports use cases such as:
     - **Running applications with admin privileges.**
     - **Accessing remote desktops using Windows Security.**

---

## Resources

- [Download IDmelon Credential Provider](https://idmelon.com/docs/downloads)
- [Learn more about FIDO2 Security Standards](https://fidoalliance.org/)
