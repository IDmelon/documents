---
title: "Restrict Access from a Range of IP Addresses"
description: "Example policy that restricts access based on IP address range."
lead: ""
date: 2023-09-20T15:00:22+03:30
lastmod: 2023-09-20T15:00:22+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_examples"
weight: 53500
toc: true
---

To Restrict access from a range of IP addresses, follow these steps:

1. Go to the admin panel.
2. Under the **Security Policies** tab, select **Security key Policies**.
3. Set **Default Policy** on **Allow**.
4. In the **Security key Policies** page, click on **+ New Policy** Button.
5. In **Policy Name** and **Priority** step, fill the fields, give your policy a name that would be shown in policy list and a priority. A smaller number means higher priority.
6. In set condition step, choose **IP Restriction**, you can choose from named lists here or add a new **IP Restriction** with + button right in front of **IP Restriction**. If you choose to add **IP Restriction**, an **IP Restriction** form will pop up. You can specify IP or IP Range in the form (IP range will be defined in this format: 192.168.1.0/20, which means 20 IP from this IP as a starting point).
7. In the select action step, you can choose between two actions, **Block** or **Allow**. In case we want to restrict access from a range of IP addresses and our default policy is **Allow**, so we choose **Block** for the action. When it’s done click on Next.
8. In **Review & Confirm** you can check all inputs and verify if you did every step properly.
9. You can see the policy in the **Security key Policy** list.
