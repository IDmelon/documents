---
title: "Block Access from a Specific Type of Security Key Device"
description: "Example policy to restrict access using a type of security key (e.g., smartphone, or card)."
lead: ""
date: 2023-09-20T15:01:20+03:30
lastmod: 2023-09-20T15:01:20+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_examples"
weight: 53600
toc: true
---

To block access from a specific type of security key device, follow these steps:

1. Go to the admin panel.
2. Under the **Security Policies** tab, select **Security key Policies**.
3. Set **Default Policy** on **Allow**.
4. In the **Security key Policies** page, click on **+ New Policy** Button.
5. In **Policy Name** and **Priority** step, fill the fields, give your policy a name that would be shown in policy list and a priority. A smaller number means higher priority.
6. In set conditions step, specify the **Security key Device Type** that you want to block access. Let’s assume we want to block any access that is coming from a smartphone security key device, in this case, choose **Smartphone** in **Security key Device Type**.
7. In the select action step, you can choose between two actions, **Block** or **Allow**, in this case, we want to block access requests coming from smartphone device and our **Default Policy** is **Allow**, so choose **Block**.
8. In **Review & Confirm** you can check all inputs and verify if you did every step properly.
9. You can see the policy in the **Security key Policy** list.
