---
title: "Tying a Shared Device to a Single User Account"
description: "Example policy that ties a shared device to a single user account."
lead: ""
date: 2023-09-20T14:58:32+03:30
lastmod: 2023-09-20T14:58:32+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_examples"
weight: 53300
toc: true
---

To tie a shared device to a single user account, follow these steps:

1. Go to the admin panel.
2. Under the **Security Policies** tab, select **Security key Policies**.
3. Set the **Default Policy** on **Block**.
4. In the **Security key Policies** page, click on **+ New Policy** Button.
5. In **Policy Name** and **Priority** step, fill the fields, give your policy a name that would be shown in policy list and a priority. A smaller number means higher priority.
6. In set conditions step, you should specify the **User** you want to bound to the shared device.
7. Also, in set condition step, you need to choose the device you want the selected user to be bound to.
8. In the select action step, you can choose between two actions, **Block** or **Allow**. In case we want to bound a device to a single user and the default policy is block, then we need to set the action to **Allow**. When it’s done click on the **Next** button.
9. In **Review & Confirm** you can check all inputs and verify if you did every step properly.
10. You can see the policy in the **Security key Policy** list.
