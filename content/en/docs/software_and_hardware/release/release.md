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

### IDmelon Panel 2.204.0

October 27, 2026

#### New Features & Improvements

- Actions menus for security keys, workflows, provisioning, and bulk actions have been fully revamped! 
- Enrollment for face security keys has been changed to mimic the same behavior as fingerprint enrollment. To use face security keys, please contact a member from IDmelon's team so they can provide the right driver and setup.

#### Bug Fixes

- Non-admin users were not being able to reset their card's PIN from their personal Panel. They should be able to do so again.
- Enrolling Crescendo cards as a hardware security key was not always going through smoothly. Smooth sailing from now on.
- Deleting a pending security key was not automatically invalidating the registration link for that security key. This behavior has been fixed.
- Users who were not receiving invitation emails for hardware security keys will be receiving them again.

### IDmelon Panel 2.203.0

October 15, 2026

#### New Features & Improvements

- Want users to change their PIN after their first usage? This feature has been added with the "Require PIN change on first use" action which can be found under the general **Security Keys** tab and the user's **Security Keys** tab as well. For this, it is required to have an Accesskey version 3.15.3.7 or higher.
- Users are now able to access the IDmelon Panel with an email link instead of an OTP code. Activate this option by navigating to **Authentication** > **Authentication Profile** in the IDmelon Panel and checking the Magic Link option.
- After registering a badge or face security key using the activate now option, admins can decide to assign them a PIN, choose their own PIN, or get a random PIN.
- Automatic Microsoft Graph API permissions acceptance flow has been removed. Microsoft Graph API permissions are to be configured manually under **App Integrations** > **API**.

#### Bug Fixes:

- Users with no devices registered where not showing in the pie charts under the **Home** tab. This issue has been fixed.

### IDmelon Panel 2.198.3

September 29, 2026

#### Bug Fixes:

- Small bug fixes and improvements to keep IDmelon running smoothly.

### IDmelon Panel 2.198.0

September 22, 2026

#### New Features & Improvements

- The self-service portal can now be used for face, fingerprint, smartphone, and hardware security keys. Contact IDmelon to add these options. To learn more about how to use these, check our [self-service documentation](/docs/for_administrators/users_and_security_keys_management/security_key_enrollment_flows/self_service_enrollment_flow/).
- There are new options for the user identity verification step in the self-service. Nametag can be used for government ID verification [(docs)](/docs/for_administrators/authentication/identity_providers/nametag/), or Entra ID can be set up as a verfication step [(docs)](/docs/for_administrators/authentication/identity_providers/entra_id_oidc/).
- Activate via email option for face and fingerprint security keys has been added
- Now, Entra device groups can be used for conditional access policies without the need to use named lists.

#### Bug Fixes

- Updating devices imported from Entra ID would sometimes return an error. This has been fixed.
- Fixes for IDmelon custom SAML SSO applications were made.

### IDmelon Panel 2.193.2

August 27, 2026

#### New Features & Improvements

- Dashboard + Home = Our new Panel landing page. Find user analytics, new provisioning tasks' status, and quick actions from your Panel's home page.
- Activity logs have a new look. Find more information about your users' activity logs by clicking on the plus (+) icon to the left of the log record.
- Like activity logs, passkeys have a plus (+) icon to the left of the record with more information on the passkey.

#### Bug Fixes

- Card activation via email workflows were failing to send emails. This issue has been fixed.
- The Microsoft passkey provisioning workflow was not activating for users not imported from Entra. The workflow will now run for all users regardless of where they were imported.
- Small fixes to security keys of Platform type have been made.

### IDmelon Panel 2.192.4

August 18, 2026

#### Bug Fixes

- Fixes to Microsoft passkeys for Entra GCC High environments.
- Password autofill for shared mobile has been fixed.
- Admins of multiple Organization Units can be easily identified as roles are shown in separate rows.

### IDmelon Panel 2.192.0

July 29, 2026

#### New Features & Improvements

- Looking to export authentication logs in CSV or PDF formats? Admins can now do this from the /dashboard/users and /dashboard/credential-reports pages.
- When a Microsoft passkey is deleted from IDmelon, this one is automatically deleted on Entra ID.
- Improvements to use passkeys offline have been implemented.
- Improvements to the new Provisioning tab have been made to better steamline user onboarding and passkey provisioning.
- IDmelon now supports integrations with Entra GCC High environments

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
