---
title: "RDP Sign-in Experience with FIDO2 Security Key"
description: "Sign in to Windows via Remote Desktop (RDP) using your FIDO2 security key."
lead: ""
date: 2025-01-15T15:35:46+03:30
lastmod: 2025-01-15T15:35:46+03:30
draft: false
images: []
menu:
  docs:
    parent: "windows_passwordless_deployment"
weight: 89000
toc: true
---

The IDmelon FIDO2 Security Key offers a seamless and highly secure experience for accessing remote desktop sessions over RDP. By leveraging FIDO2 security protocols, users can authenticate with modern cryptographic methods that eliminate the need for traditional passwords, enhancing security and user convenience.

This innovative approach works across a wide variety of devices, including thin clients, and ensures that users can maintain secure access even when operating within a remote desktop session. For example, a user can use their IDmelon FIDO2 Security Key to sign into a web application while inside a remote desktop session, providing a consistent and secure experience across environments.

---

## Requirements

To set up and use the IDmelon FIDO2 Security Key for remote desktop sessions, ensure the following requirements are met:

1. **IDmelon FIDO2 Security Key Enrollment:**
   - Users must have their IDmelon FIDO2 Security Key (e.g., NFC/RFID Employee Badge) enrolled and ready for use.

2. **IDmelon FCP Installation:**
   - Install the IDmelon FIDO2 Credential Provider (FCP) on the host system to enable remote session access via RDP.  
   - Download the FCP from the [IDmelon Documentation Downloads page](https://idmelon.com/docs/downloads).

3. **Key-Stroking Mode Configuration:**
   - Configure your reader to enable key-stroking mode. If you are using an **rf IDEAS reader**, follow these steps:
     - Use the **rf IDEAS Configuration Utility** to set the reader to key-stroking mode.
     - After configuration, execute the following shell command in the Command Prompt to enable the credential provider for key-stroking mode:

       ```shell
       idmelonfcp key-stroking -e
       ```

4. **IDmelon WebLogin Extension:**
   - For using the FIDO2 Security Key within the remote session for web authentication, install the **IDmelon WebLogin Extension** for Chrome or Edge.  
   - Download it from the [Chrome Web Store](https://chromewebstore.google.com/detail/idmelon-weblogin/eagmgpbjpedchliifpgfgogdknnmkaej).

---

## User Experience

The IDmelon FIDO2 Security Key enhances the user experience by offering secure and efficient authentication for remote desktop sessions. Below is an overview of the experience:

![alt](/images/vendor/gifs/rdp-login.gif)

1. **Initiating the Session:**
   - The user connects to the remote desktop session from a Linux-based thin client using RDP.
   - Upon reaching the sign-in page, the IDmelon FIDO2 Credential Provider (FCP) is pre-selected, waiting for user input.

2. **Authentication Process:**
   - The user taps their **NFC/RFID Badge** on the reader.
   - The IDmelon FCP captures the card information in key-stroking mode and prompts the user to enter their **Security Key PIN** to complete the FIDO2 authentication.

3. **First-Time Login:**
   - For the initial login, the user is required to enter their **Active Directory (AD) credentials**.
   - Subsequent logins will only require the **Security Key PIN**, streamlining the process.

4. **Successful Authentication:**
   - Once authenticated, the user gains access to the remote session via RDP, using their **NFC/RFID Badge** as the security key.

---

## Key Benefits

- **Passwordless Authentication:** Eliminates the need for passwords, improving security and user experience.
- **Compatibility Across Devices:** Works with thin clients, desktops, and various operating systems.
- **Streamlined Access:** After the first login, users only need their security key PIN, reducing time and effort.
- **Enhanced Security:** Utilizes FIDO2 protocols to prevent phishing and credential theft.

For further details, visit [IDmelon Documentation Downloads](https://idmelon.com/docs/downloads) or the [Chrome Web Store](https://chromewebstore.google.com/detail/idmelon-weblogin/eagmgpbjpedchliifpgfgogdknnmkaej) to download the necessary tools.

---

By integrating IDmelon FIDO2 Security Keys into your authentication process, you can enjoy a secure and seamless experience for remote desktop sessions and beyond.
