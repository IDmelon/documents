---
title: "Target Computer IP"
description: "Limit security key access to a list of IP addresses."
lead: ""
date: 2023-09-20T14:34:48+03:30
lastmod: 2023-09-20T14:34:48+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_conditions_signals"
weight: 52400
toc: true
---

The "Target Computer IP" condition in security key policies allows administrators to set rules and restrictions based on the specific IP address of the computer or device from which a user is attempting to authenticate using their security key. This condition adds an extra layer of security by controlling access based on the IP address of the device.

Here's how the "Target Computer IP" condition works within security key policies:

- IP Address Identification: Each device connected to a network is assigned a unique IP address. This address identifies the device's location on the network and is used to route data to and from the device.
- Access Control: With the "Target Computer IP" condition, administrators can configure policies that determine whether a user can access the system using their security key from a specific IP address or range of IP addresses.
- Enhanced Security: By implementing policies based on the "Target Computer IP" condition, organizations can enhance security by ensuring that users can only access RPs from approved IP addresses.  
- Use Cases: For example, an organization might set a policy that allows users to authenticate using their security key only when they are within a company's specific office or network. This restricts access to devices located within trusted environments.

In summary, the "Target Computer IP" condition within security key policies enables organizations to define access rules and restrictions based on the IP address of the device from which a user is trying to authenticate. This approach adds an additional layer of security by ensuring that only approved IP addresses can be used for secure access, enhancing control over authentication processes.
