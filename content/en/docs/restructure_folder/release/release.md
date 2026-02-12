---
title: "Release Notes"
description: "IDmelon Release notes"
lead: ""
date: 2022-02-16T18:14:02-08:00
lastmod: 2022-02-16T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "release"
weight: 360000
toc: true

---

This file contains detailed information about each release of the project.

It serves as a log of changes made to the project over time,
providing insights into new features, improvements, bug fixes,
and other modifications introduced in each version.

## SKM

### v2.28.0 (July 29, 2024)

Improvements

- [ID-6796] Database Migration and Refactor for Tokens and Security Key Devices
- [ID-6880] Self-Service PIN Recovery

Fixes

- [ID-6885] Fix User Group Replacement Issue During Import

Features

- [ID-6590] Enhancing Email Notifications for Payment Processing Events

- [ID-6742] Add email field to assign security key API

### v2.24.0 (June 23, 2024)

Improvements

- [ID-6879] Verifying employees for self-managed workflows using Employee ID

Features

- [ID-6901] Add API Key Functionality on Backend
- [ID-6860] Implement API for IDmelon Sync to Post Users

### v2.21.0 (June 10, 2024)

Improvements

- [ID-6854] Add Export Feature to Admin Panel

Fixes

- [ID-6851] Remove IDmelon RP From the Policy Whitelist
- [ID-6853] Fix Duplicate IDmelon Passkey Issue in Pairing Tool

### v2.20.0 (May 27, 2024)

Improvements

- [ID-6715] Grouping and User Sync Functionality
- [ID-6812] Refactor Smartphone Update Status Checks

Fixes

- [ID-6818] Move Passkey's Check Policy Service to SKM
- [ID-6831] Enforce Password Reset on PIN Required Change
- [ID-6833] Add IDmelon Passkey Credential ID to Password Creation
- [ID-6735] Refactor import from SCIM to use eid instead of uid
- [ID-6832] Add Validation for Password in API Requests with UV Required

Features

- [ID-6792] Implement Phone and Email Verification
- [ID-6790] Enable Verification in Self-Enrollment

### v2.19.0 (May 13, 2024)

Improvements

- [ID-6793] Password Manager - Add and Edit Functionality
- [ID-6807] Update User Status on Password Addition
Fixes

- [ID-6798] Resolve Phone Number Deletion Issue
- [ID-6799] Backend API Fix Edit One User

### v2.18.3 (May 5, 2024)

Improvements

- [ID-6765] Merge CSV Import Options for Admin Panel
- [ID-6768] Scale Testing for Panel and System Functionality
- [ID-6766] Merge Super Admin and Admin Import CSV Controllers
- [ID-6770] Optimize User Loading Process

### v2.18.0 (April 30, 2024)

Improvements

- [ID-6722] Migration of Password Encryption Method

Features

- [ID-6732] Add remove functionality to remote groups - backend

Fixes

- [ID-6726] Policies should not block idmelon.com rp logins

## Panel

### v2.21.0 (July 29, 2024)

Improvements

- [ID-6878] Workflows for Security keys view
- [ID-6880] Self-Service PIN Recovery

Fixes

- [ID-6788] Resolve Fingerprint Re-enrollment Issue
- [ID-6906] Save Button in PIN Settings Gets Disabled

Features

- [ID-6898] Kiosk company homepage

### v2.20.0 (June 23, 2024)

Improvements

- [ID-6855] Enable Admin to Filter Pending Users
- [ID-6879] Verifying employees for self-managed workflows using Employee ID

Features

- [ID-6902] Add API Key Section in Settings Page

Fixes

- [ID-6876] Reset pin button works even when it's disabled

### v2.18.0 (June 10, 2024)

Improvements

- [ID-6829] Entra ID Import Users: Ask to choose groups and Cancel option
- [ID-6849] Replace provisioning:// Deeplink with idmelonprovisioning:// in Passkey Provisioning
- [ID-6854] Add Export Feature to Admin Panel
- [ID-6855] Enable Admin to Filter Pending Users

Fixes

- [ID-6798] Resolve Phone Number Deletion Issue
- [ID-6800] Panel Interface Update Edit User

### v2.14.0 (May 27, 2024)

Improvements

- [ID-6809] Add 'Issue new invoice' Link to Billing Status Page

Fixes

- [ID-6831] Enforce Password Reset on PIN Required Change
- [ID-6843] Fix Users' Status Icons in Shared With Dialog

Features

- [ID-6791] Integrate CAPTCHA into Self-Enrollment
- [ID-6834] Require Password Input on PIN Required Change

### v2.13.0 (May 13, 2024)

Improvements

- [ID-6775] Update Panel: Replace "Local" and "Remote" with "Internal" and "External" for Groups
- [ID-6793] Password Manager - Add and Edit Functionality

Features

- [ID-6794] Implement Add and Edit Password Functionality in Panel

### v2.12.2 (May 5, 2024)

Improvements

- [ID-6765] Merge CSV Import Options for Admin Panel
- [ID-6768] Scale Testing for Panel and System Functionality
- [ID-6767] Simplify CSV Import Checkbox
- [ID-6769] Optimize Data Loading Sequence for Statistics Page

### v2.12.0 (May 1, 2024)

Features

- [ID-6733] Add remove functionality to remote groups - front
- [ID-6737] Add search functionality for groups - front
- [ID-6758] Remove Add Password Functionality From Panel
- [ID-6752] Create Get Tokens Page

Fixes

- [ID-6718] Allow user to type the country code without going over the list
- [ID-6721] Super Admin Panel Select Date
- [ID-6740] Fix RP Icon in Passkeys List
- [ID-6744] Prevent Scrolling on Sign-Up Page

## Passkey

### v2.23.0 (July 22, 2024)

Improvements

- [ID-6880] Self-Service PIN Recovery

### v2.22.4 (May 27, 2024)

Improvements

- [ID-6793] Password Manager - Add and Edit Functionality
- [ID-6840] Set Default Values for Options in GetAssertion Requests
- [ID-6807] Update User Status on Password Addition

Fixes

- [ID-6818] Move Passkey's Check Policy Service to SKM

### v2.18.0 (April 30, 2024)

Fixes

- [ID-6726] Policies should not block idmelon.com rp logins

## Billing

### v1.15.0 (July 22, 2024)

Features

- [ID-6590] Enhancing Email Notifications for Payment Processing Events
