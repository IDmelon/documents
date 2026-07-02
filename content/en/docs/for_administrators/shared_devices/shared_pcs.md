---
title: "Shared PCs"
description: ""
lead: ""
date: 2023-09-20T15:18:44+03:30
lastmod: 2023-09-20T15:18:44+03:30
draft: false
images: []
menu:
  docs:
    parent: "device_management"
weight: 61000
toc: true
---

## Shared Workstations with Individual Accounts

For environments where individual accounts are used in shared PCs, no extra configurations are required outside of IDmelon's regular security key and passkey onboarding processes. All that is needed for a user to log in to a shared workstation with their own account is to have their existing identifier enrolled into IDmelon and their passkey. Because IDmelon turns your existing identifiers into security keys, there is no need to onboard every user in each device in their environment, and there is no limit to the number of users that can use each device. Thus,

- After onboarding, users can access all devices within the environment with their individual account
- There is no individual onboarding needed for every device
- There is no limit in the number of users allowed to log in to a workstation

> **Note**: For on-prem AD environments using IDmelon's FCP, users are required to enter their password the very first time that they use their security key to log in. After this, the user does not have to re-enter their password for any other workstation in the environment.

## Shared Workstations with Generic Accounts

When using shared or generic accounts, it is common for multiple users to access different shared accounts from various workstations. This scenario is illustrated in the diagram below.

![Shared Account Diagram](/images/vendor/shared_pcs/shared_account_01.svg)

Traditional shared credentials, such as common passwords, make security and traceability a living nightmare. IDmelon eliminates these problems by
1) Allowing users to use their own credentials to access shared accounts, and
2) Creating a log trail that shows the credential used to access the shared account at that point in time

To set up a shared account, please refer to [this document](/docs/for_administrators/users_and_security_keys_management/configure_a_shared_user_account/setup_a_shared_account/).

After setting a up a shared account, please follow [this document](/docs/for_administrators/users_and_security_keys_management/configure_a_shared_user_account/manage_the_use_of_a_shared_security_key/) to ensure that users access the correct shared account in the right workstation. 

> **Note**: For on-prem AD environments using IDmelon's FCP, it is recommended for admins to set the shared account's password in the IDmelon Panel. This way, users do not have to enter the shared account's password themselves.