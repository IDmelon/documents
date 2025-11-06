---
title: "Shared Device Mode Configuration Using Jamf"
description: ""
lead: ""
date: 2023-09-20T15:19:33+03:30
lastmod: 2023-09-20T15:19:33+03:30
draft: false
images: []
menu:
  docs:
    parent: "shared_mobile_devices"
weight: 62200
toc: true
---

**Jamf** is a trusted solution for managing iPads in various organizations, including businesses, schools, and hospitals. With over 70,000 customers, Jamf provides robust iPad management capabilities.

## Prerequisites

- To use the shared iPad feature, the OS version of iPads must be **17 or later**.
- IDmelon Authenticator app must be added to **Jamf Pro**. In case you are having issues with that, contact **Jamf Support**.

## How to Configure

1. Configure **General**, **Scope** and **Self-Service** according to your organization's policies.
2. Select the **App Configuration** tab from the menu, enter the following configuration as shown in the image below, and then click the **Save** button.

```xml
<dict>
  <key>shared_device_passkeys</key>
  <true/>
  <key>authentication_type</key>
  <string>onInit</string>
  <key>device_id</key>
  <string>$UDID</string>
  <key>api_key</key>
  <string>[API_KEY]</string>
</dict>
```

**One-Time Use Passkeys:**

Add the following key to the above configurations to automatically log the user out after their first login with the passkey.

```xml
<key>one_time_use_passkeys</key>
<true/>
```

**Dedicated Deployments Base API URL:**

If you are using dedicated deployment, such as on-premises, add the following key-value to the configurations.

```xml
<key>base_api_url</key>
<string>https://example.com/api/url</string>
```

**Self Service URL:**

By adding the self-service URL (available in the [IDmelon Admin Panel](https://panel.idmelon.com) > Security Keys > Workflows > Self-Service Actions) to the configurations, users will be redirected to the enrollment page if their card is not yet registered.

```xml
<key>self_service_url</key>
<string>https://panel.idmelon.com/self-service/{UID}</string>
```

**Auto Logout:**

By adding this configuration, users will be automatically logged out of the app after a specified time or after the first use of the passkey.<br>
*Allowed values: one-time use, 5m, 60m, 2h, 4h, 6h, 8h*

```xml
<key>auto_logout</key>
<string>2h</string>
```

***Note**: The value of the **authentication_type** depends on the **Card Verification Method** that your organization's admins have set in the IDmelon admin panel.*

**Allowed values for authentication_type:**

- **onInit**: User is required to enter PIN once only after tapping the card on a reader.
- **onUse**: User is required to enter PIN for each login.
- **none**: The login process is done without entering any PIN (PINless mode).

![Jamf Panel](/images/vendor/shared_ipads/jamf_panel_app_config.png)
Finally, you should see the **IDmelon Authenticator** app in the Mobile Device Apps.
![Jamf Panel](/images/vendor/shared_ipads/jamf_panel_added_app.png)

## Connect to the Organization

After adding the IDmelon Authenticator to the Jamf Pro panel, the application will be available on iPads as shown in the image below (or it can be installed through **Self Service** app).
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_idmelon_app.PNG)

Once the IDmelon Authenticator app opens, the activation process will be completed automatically.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_shared_device_mode.PNG)
If everything goes well, you will see the following view.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_tap_card.PNG)

## End User Experience

The login steps are as follows: at the beginning of the shift, an employee will first log in with their card, and at the end of the shift, by exiting the application, everything will be ready for the next person.

1. Open the **IDmelon Authenticator**.
2. Get close to the reader and tap your card on it.
3. According to the **Card Verification Method** set in the IDmelon admin panel, if PIN is required, enter it. Otherwise, go to the next step.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_enter_pin.PNG)
4. In case of successful login, user information will be displayed. At this stage, move the app to the background.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_logged_in.PNG)
5. Open the **Jamf Setup** app and tap on the **Sign In** button.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_jamf_setup.PNG)
6. Select the **Sign-in options**.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_ms_authenticator1.PNG)
7. Select the **Face, fingerprint, PIN or security key**.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_ms_authenticator2.PNG)
8. Tap the **Continue**.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_passkey.PNG)
9. If the login operation is successful, you will see the following view, which can be different depending on your configuration in Jamf.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_jamf_setup_logged_in.PNG)

### Logout Experience

At the end of the shift, first, open the **IDmelon Authenticator** and then tap the logout icon on the top right corner of the app. Your user information and existing passkeys will be deleted from the iPad.
![Shared iPad](/images/vendor/shared_ipads/shared_ipad_logout.PNG)

If **Soft Reset** is enabled in **Jamf Reset**, to completely log out of all your accounts, first open one of the Microsoft applications (for example, MS Teams) and log out of it. After that, all your sessions and cookies will be completely deleted.
