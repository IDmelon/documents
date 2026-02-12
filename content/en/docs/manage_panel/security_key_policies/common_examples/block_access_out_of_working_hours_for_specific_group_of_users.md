---
title: "Block Access out of Working Hours for Specific Group of Users"
description: "Example policy to block access outside working hours for a specific user group."
lead: ""
date: 2023-09-20T14:57:41+03:30
lastmod: 2023-09-20T14:57:41+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_examples"
weight: 53200
toc: true
---

To Block access out of working hours for specific group of users, follow these steps:

1. Go to the admin panel.
2. Under the **Security Policies** tab, select **Security key Policies**.
3. Put default policy on **Allow**.
4. In the **Security key Policies** page, click on **+ New Policy Button**.
5. In **Policy Name** and **Priority** step, fill the fields, give your policy a name that would be shown in policy list and a priority. A smaller number means higher priority.
6. In set conditions step, you should specify your **Security key Device Type**, you can choose whatever device that you want for time constraint.
7. In set condition step, choose the **Groups** you want to block their access out of working hours. Also, choose **Time limits**, you can choose from named list here or add a new Time Limits with + button right in front of Time Limits. If you choose to add **Time Limits**, a Time-based Limitation form will pop up. You can specify **Time Zone**, **Days of Week** and **Time Period**. For example, in this case you can choose Monday to Friday for days of the week and 9 to 5 in the time period. Whenever you are done, click on **Add Time** to add that condition then click on **Next** button.
8. In the select action step, you can choose between two actions, **Block** or **Allow**. In case we want to block a group of out of working hours and the default policy is **Allow**, so we choose **Block** to do this. When it’s done click on **Next**.
9. In **Review & Confirm** you can check all inputs and verify if you did every step properly.
10. You can see the policy in the **Security key Policy** list.
