---
title: "Provision FIDO2 Passkeys for Other RPs"
description: ""
lead: ""
date: 2025-04-15T15:00:00+03:30
lastmod: 2025-04-15T15:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "manage_passkeys_and_credentials"
weight: 34200
toc: true
---

The IDmelon Provisioning Extension is used to register passkeys for non-Microsoft RPs that support the FIDO2 protocol.

## Prerequisites

- FIDO2 Security Key must be enabled in Entra ID for the targeted users.
- Use the latest version of Chrome or Edge.
- Install the **IDmelon Provisioning extension** from the Chrome Web Store.
- Enable **Incognito/InPrivate** mode: chrome://extensions/?id=jpiokfcldcenkkhhhkeklechbenokmfo

## Assigning Passkeys through IDmelon Provisiong Extension

1. Log in to the **IDmelon Admin Panel**.
2. Navigate to **Users → All Users**.
3. Select the target user.
4. Go to the **Passkeys** tab and click **Add Passkey → Other RPs**.
5. Follow the instructions in the prompt window and click **Next**.
6. A new incognito window opens.
7. Navigate to a FIDO2-supported website and add a passkey/security key.
8. The extension captures and assigns the passkey automatically to the user.

## Additional Notes and Troubleshooting

- **User Doesn’t See Security Key Option on Microsoft Login:**
  - Ensure FIDO2 is enabled in Entra ID.
  - Verify passkey assignment in IDmelon Admin Panel.
- **Provisioning Extension Not Capturing Passkey:**
  - Confirm **FIDO2** support on the target site.
  - Make sure extension is allowed in Incognito/InPrivate.
  - Check logs in the extension.
- **User Cannot Log in to Windows Using Security Key:**
  - Make sure the device is Azure AD joined, or Hybrid joined.
  - Ensure the security key option is enabled for Windows login.
  - If the security key option is not available on the Windows login screen, follow these steps to enable it: [Enable Security Key for Windows Login](https://learn.microsoft.com/en-us/entra/identity/authentication/howto-authentication-passwordless-security-key-windows).
