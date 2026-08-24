---
title: "IDmelon Panel Release Notes"
description: "IDmelon Panel Release notes"
lead: ""
date: 2022-02-16T18:14:02-08:00
lastmod: 2026-06-15T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "release"
weight: 360000
toc: true

---

### IDmelon Panel 2.192.4

August 18, 2026

#### Bug Fixes

- Fixes to Microsoft passkeys for Entra GCC High environments.
- Password autofill for shared mobile has been fixed.
- Admins of multiple Organization Units can be easily identified as roles are shown in separate rows.

### IDmelon Panel 2.192.0

July 29, 2026

#### New Features & Improvements

- IDmelon now supports integrations with Entra GCC High environments.
- Looking to export authentication logs in CSV or PDF formats? Admins can now do this from the /dashboard/users and /dashboard/credential-reports pages.
- When a Microsoft passkey is deleted from IDmelon, this one is automatically deleted on Entra ID.
- Improvements to use passkeys offline have been implemented.
- Improvements to the new Provisioning tab have been made to better steamline user onboarding and passkey provisioning.

#### Bug Fixes

- Smartphone security key activation via identity verification was failing. This issue has now been fixed.
- Passkey provisioning workflows were unexpectedly showing a "user not found" error for users outside of the workflow group. This issue has been fixed.
- In-app purchases from IDmelon Authenticator app for personal accounts has been fixed after payments were not being validated correctly.

### IDmelon Panel 2.186.0

July 6, 2026

#### New Features

- A new tab has appeared! Manage all workflows from the Provisioning tab to streamline user onboarding and passkey provisioning.
- Polski? Deutsch? Now, self-service customization supports these languages as well.

#### Bug Fixes

- Discrepancies between number of security keys shown in the Users tab and the actual number of security keys has been fixed.

### IDmelon Panel 2.183.0

June 25, 2026

#### New Features

- User lifecycle management just got a lot easier! Now, admins can set their Panel's SCIM deprovisioning policy to suspend disabled users and re-activate them once re-enabled in Entra/Okta. Users suspended via SCIM will remain suspended in the IDmelon Panel for a configurable period of time before they are permanently deleted
- Predefined admin roles are now available on IDmelon! These can be assigned through SCIM ([docs](/docs/for_administrators/users_and_security_keys_management/add_or_import_users/import_users_with_scim/sso_scim_synchronization_with_office/#assigning-administrator-roles-via-scim)) or manually from the Panel itself
- Instead of adding workspace administrators one by one, admin rights can now be assigned to a group of users

### IDmelon Panel 2.178.0

June 15, 2026

#### New Features

- When using SCIM, admins can easily deprovision users imported and managed outside of SCIM's scope.
- Contact customer support to set up security key enrolment policies which allow, block, or automatically replace security keys.

#### Bug Fixes

- Trouble adding a credit card? We have improved the credit card addition flow.

### IDmelon Panel 2.175.0

June 2, 2026

#### New Features

- Find the self-service language selector on all pages of the self-service to easily switch languages.
- Card invitation email links now sends users to the self-service page, following user's preferred language.

### IDmelon Panel 2.173.0

May 27, 2026

#### New Features

- Español? Français? 中 文? Nederlands? Now, self-service customization supports all of these languages.
- IDmelon now syncs the preferred language attribute from Entra ID to IDmelon. This will make customized emails, SMS messages, and self-service pages default to the user's preferred language.

### IDmelon Panel 2.167.0

May 20, 2026

#### Bug Fixes

- Small updates and bug fixes that help our systems continue to perform.

### IDmelon Panel 2.160.4

April 24, 2026

#### Bug Fixes

- Email activation links from the security key device assignment workflow for smartphones were in need of some maintenance. Now, the workflow will run smoothly, as it normally does.

### IDmelon Panel 2.160.3

April 21, 2026

#### Bug Fixes

- Shared account login option not showing up during login attempts? Now, unless there is a policy in place or you are using the WebLogin Extension, both personal and shared account login options should always be available.
- Passkeys on the IDmelon Authenticator app for Android no longer perform magic tricks where they disappear, only to reappear after refreshing the app. Passkeys now stay visible at all times in your IDmelon app for Android.
