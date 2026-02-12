---
title: "Target Computer Name"
description: "Limit security key access to a list of devices."
lead: ""
date: 2023-09-20T14:34:08+03:30
lastmod: 2023-09-20T14:34:08+03:30
draft: false
images: []
menu:
  docs:
    parent: "common_conditions_signals"
weight: 52300
toc: true
---

The "Target Computer" condition in security key policies refers to the ability to set rules and restrictions based on the specific computer or device from which a user is attempting to authenticate using their security key. This condition allows administrators to control and customize access based on the originating device, adding an extra layer of security and control to the authentication process.

Here's how the "Target Computer" condition works within security key policies:

- Device Identification: Each device, such as a computer or workstation, has a unique identifier (name) that can be recognized by the system. This identifier is used to differentiate one device from another.
- Access Control: With the "Target Computer" condition, administrators can configure policies that dictate whether or not a user can access a particular computer or an RP using their security key from a particular computer.  
- Enhanced Security: By implementing policies based on the "Target Computer" condition, organizations can enhance security by ensuring that users can only access approved computers or RPs from approved computers. This prevents unauthorized access from/to computers that are not recognized or trusted.
- Use Cases: For instance, an organization might configure a policy that allows users to authenticate using their security key only from computers that are within the company's physical premises or within the corporate network. This way, even if a user's security key is compromised, the potential for unauthorized access is limited to approved computers.

In summary, the "Target Computer" condition within security key policies enables organizations to set access rules and restrictions to/from computer which a user is attempting to authenticate. This approach provides an additional layer of security by ensuring that only approved computers can be used for secure access, enhancing control over authentication processes.
