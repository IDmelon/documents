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

In environments where multiple users log in to different PCs with their own account, it can be troublesome to set up all. Regardless of the environment, on-prem AD, hybrid, or fully Entra-joined, IDmelon makes it so there is n

## Shared Workstations with Generic Accounts

When using shared or generic accounts, it is common for multiple users to access different shared accounts from various workstations. This scenario is illustrated in the diagram below.

![Shared Account Diagram](/images/vendor/shared_pcs/shared_account_01.svg)

Traditional shared credentials, such as common passwords, make security and traceability a living nightmare. IDmelon eliminates these problems by
1) Allowing users to use their own credentials to access shared accounts, and
2) Logging all login activities based on the credential used to access the shared account.

To set up a shared account, please refer to [this document](/docs/for_administrators/users_and_security_keys_management/configure_a_shared_user_account/setup_a_shared_account/).

After setting a up a shared account, please follow [this document](/docs/for_administrators/users_and_security_keys_management/configure_a_shared_user_account/manage_the_use_of_a_shared_security_key/) to ensure that users access the correct shared account in the right workstation. 
