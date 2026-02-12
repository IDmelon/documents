---
title: "Allow Access Just from a City or Country"
description: "Limit user access using certain security keys to a city or country."
lead: ""
date: 2023-09-20T14:56:34+03:30
lastmod: 2023-09-20T14:56:34+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_examples"
weight: 53100
toc: true
---

To Allow access just from a city or country follow these steps:

1. Go to the admin panel.
2. Under the **Security Policies** tab, select **Security key Policies**.
3. Set the **Default Policy** on **Block**.
4. In **Security key Policies** page, click on **+ New Policy Button**.
5. In **Policy Name** and **Priority** step, fill the fields, give your policy a name that would be shown in policy list and a priority. A smaller number means higher priority.
6. When setting conditions, you should specify your **Security key Device Type**. In this case, if you want your coworkers to access just from a specific location, you should choose smartphone as the security key device.
7. Also, in setting conditions, choose **Location Restriction**, you can choose from named list here or add a new location restriction with + button right in front of Local Restriction. If you choose to add location you can move the circle to bound the location, you can also reduce or increase the radius of location circle. Whenever you are done setting the conditions, you can click on the **Next** button to go to the next step.
8. In the select action step, you can choose between two actions, **Block** or **Allow**, in this case we want to bound some users to login from a specific location and our default policy is **Block**, so we choose **Allow**. When it’s done click on the **Next** button.
9. In **Review & Confirm** you can check all inputs and verify if you did every step properly.
10. You can see the policy in the list of **Security key Policies**.
