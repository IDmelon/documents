---
title: "Time of Use"
description: "Restrict security key usage to certain times of the day."
lead: ""
date: 2023-09-20T14:33:37+03:30
lastmod: 2023-09-20T14:33:37+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_conditions_signals"
weight: 52100
toc: true
---

The "Time of use" condition in security key policies refers to a rule that restricts the usage of security keys to specific time periods. This means that users will only be able to use their security keys to access a system or application during designated time frames, and access will be denied outside of those defined periods.

For example, let's say an organization's security policy includes a "Time of use" condition that allows users to use their security keys for authentication only between 9:00 AM and 6:00 PM on weekdays. Outside of this time range, users attempting to authenticate using their security keys will be denied access. This can be particularly useful for ensuring that security key-based authentication is limited to working hours or other predetermined time periods, effectively reducing the risk of unauthorized access during off-hours.

Implementing a "Time of use" condition can help organizations enhance security by preventing access to critical systems during times when there might be reduced monitoring or higher risk of unauthorized usage. However, it's important to strike a balance between security and user convenience, as overly restrictive time-based policies could impact legitimate users who require access outside of regular hours.

In summary, the "Time of use" condition within security key policies is a tool to control when users are allowed to use their security keys for authentication, adding an extra layer of security to the overall access control strategy.
