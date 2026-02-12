---
title: "Configuring Policies for Shared Accounts"
description: "Configure allow and block policies for shared accounts and restrict access on specific devices."
lead: ""
date: 2023-09-11T15:05:53+03:30
lastmod: 2023-09-11T15:05:53+03:30
draft: false
images: []
menu:
  docs:
    parent: "configure_a_shared_user_account"
weight: 35300
toc: true
---

Policies allow administrators to control and restrict access to shared accounts on specific devices. By setting allow and block policies, you can ensure that only authorized accounts are permitted to log in.

## Allow and Block Policy

1. Navigate to **Conditional Access > Security Key Policies** and click **+ New Policy**.
2. Name the policy (e.g., **Allow Policy**) and set it as the highest priority.
3. On the next page:
   - Select **Card** as the Security Key Type.
   - Under **Users and Groups**, choose the shared account.
   - Under **Access Restriction on PCs**, select the target shared PC.
   - Set the action to **Allow**.
4. Submit and confirm.

Another record as Block is required to deny any other user account on the target shared PC.

1. Navigate to **Conditional Access > Security Key Policies** and click **+ New Policy**.
2. Name the policy (e.g., **Block Policy**) and assign the next priority.
3. On the next page:
   - Select **Card** as the Security Key Type.
   - Leave **Users and Groups** empty.
   - Under **Access Restriction on PCs**, select the target shared PC.
   - Set the action to **Block**.
4. Submit and confirm.

---

## Policy Features

By defining conditions such as time restrictions, location-based access, and authentication methods, administrators can enhance security and compliance for shared accounts.

For more information and examples, refer to the policies section.
