---
title: "Relying Party (or Web)"
description: "Control user access with their security keys to a list of web applications using security key policies."
lead: ""
date: 2023-09-20T14:35:13+03:30
lastmod: 2023-09-20T14:35:13+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_conditions_signals"
weight: 52500
toc: true
---

The "Relying Party" condition in security key policies allows administrators to set rules and restrictions based on the specific relying party (RP) that a user is trying to access using their security key. This condition enables organizations to control access to different RPs.

Here's how the "Relying Party" condition works within security key policies:

- Understanding Relying Parties: In the context of authentication, a relying party (RP) is a web application or service that requests user authentication from an identity provider. The RP relies on the identity provider to authenticate users before granting access to its resources.
- Access Control: With the "Relying Party" condition, administrators can configure policies that determine whether a user's security key can be used to access specific relying parties.
- Enhanced Security: By implementing policies based on the "Relying Party" condition, organizations can enhance security by ensuring that users can only use their security key to access approved RPs.
- Use Cases: For example, an organization might configure a policy that allows users to authenticate using their security key for some RPs, but not for all the available ones.

In summary, the "Relying Party" condition within security key policies empowers organizations to set access rules and restrictions based on the specific RP that a user is attempting to access. This approach adds a contextual layer of security by ensuring that users can only use their security key for approved RPs, contributing to a more customized and secure authentication experience.
