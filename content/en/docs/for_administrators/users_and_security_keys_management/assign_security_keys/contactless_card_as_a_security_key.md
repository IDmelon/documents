---
title: "Contactless Card (Badge) as a Security Key"
description: ""
lead: ""
date: 2025-04-15T15:00:00+03:30
lastmod: 2025-04-15T15:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "assign_security_keys"
weight: 33100
toc: true
---

Contactless cards (badges) are supported as FIDO2 security keys for secure authentication.

## Prerequisites

Before using a contactless card as a security key:

- A **card reader** must be connected to the user's PC.
- The compatible **IDmelon Reader Driver** must be installed. It is available for download on the [IDmelon website](https://idmelon.com/docs/downloads).
- The badge must be supported by the card reader.

## Supported Scenarios

Contactless cards are a strong fit for badge-based environments where users already carry a physical access card and need a familiar authentication method for supported systems.

Common scenarios include:

- Shared or frontline workstations where users tap a badge to authenticate.
- Environments that want to align physical access badges with digital authentication.
- Deployments that require a portable FIDO2 security key without asking users to carry an additional token.

## PIN Management

Both admins and users can reset or change the PIN associated with the contactless card at any time:

- **From the user's device:** Go to Windows Settings > Sign-in Options > Security Key > Manage to update the PIN.
- **From the IDmelon Admin Panel:** Navigate to Users > User Profile > Security Keys, click the three dots next to the assigned card, select **Reset PIN**, and enter the new PIN.
