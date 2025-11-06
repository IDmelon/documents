---
title: "Temporary Session Experience on Shared iPad"
description: ""
lead: ""
date: 2023-09-20T15:19:33+03:30
lastmod: 2023-09-20T15:19:33+03:30
draft: false
images: []
menu:
  docs:
    parent: "shared_mobile_devices"
weight: 62300
toc: true
---

In this mode, a user initially signs in to the iPad as a Guest. Once inside the IDmelon Authenticator app, the user completes login with a passkey or via MSAL login. At the end of the session, pressing the Sign Out button will erase all local data and sessions from the iPad, ensuring the device is clean and ready for the next user.

## References for Configuration

**Apple Documentation:**

- [Shared iPad overview](https://support.apple.com/guide/deployment/shared-ipad-overview-dep9a34c2ba2/web)
- [Shared iPad - enrolment and configuration](https://education.apple.com/resource/250010106)

**Microsoft Documentation:**

- [Configure shared iPad](https://learn.microsoft.com/en-us/intune/intune-service/enrollment/device-enrollment-shared-ipad#configure-shared-ipad)
- [Configure temporary sessions](https://learn.microsoft.com/en-us/intune/intune-service/enrollment/device-enrollment-shared-ipad#configure-temporary-sessions)

## Login Experience

- Log in to the iPad as a Guest.
![Login as guest](/images/vendor/shared_ipads/shared_ipad_temporary_experience_guest_login.png)

- Open IDmelon Authenticator. The app will automatically activate and be ready to use if the configurations are set up correctly.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_shared_device_mode.PNG)

- Bring the iPad close to the reader (or connect the keystroke reader).
- Tap a card on the reader.
- Enter the PIN.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_enter_pin.PNG)

- If you configured the Authenticator app to log in via MSAL, wait for the MSAL prompt and select the Continue and Next buttons.
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_1.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_2.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_3.PNG)
![MSAL login](/images/vendor/shared_ipads/shared_ipad_login_msal_4.PNG)

- Move the app to the background.
- Open any app you want to sign in to (for example, Teams).
- Paste the email using the paste icon on the keyboard.
- At the end of the shift, log out of the guest user so the iPad is ready for the next user.
![Logout guest](/images/vendor/shared_ipads/shared_ipad_temporary_experience_guest_logout.png)
