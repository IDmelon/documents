---
title: "Self-Service Enrollment Flow"
description: ""
lead: ""
date: 2026-05-26T00:00:00+00:00
lastmod: 2026-05-26T00:00:00+00:00
draft: false
images: []
menu:
  docs:
    parent: "security_key_enrollment_flows"
weight: 33510
toc: true
mermaid: true
---

This page describes how to build a self-service enrollment flow that supports user-driven provisioning.

## Enrollment Flow Design

{{< mermaid >}}
flowchart LR
    P1["Phase 1: Identify the User"]
    P2["Phase 2: Verify the User"]
    P3["Phase 3: Configure PIN"]

    P1 --> P2 --> P3

    style P1 fill:#e8f7ee,stroke:#91c7a1,stroke-width:2px
    style P2 fill:#fff3dd,stroke:#f0c36d,stroke-width:2px
    style P3 fill:#fde5e5,stroke:#ef8c8c,stroke-width:2px
{{< /mermaid >}}

In most deployments:

- **Phase 1** uses one or more identifiers to uniquely match the user.
- **Phase 2** confirms the matched user before the security key is provisioned.
- **Phase 3** is shown only when the applied security key policy requires a PIN.

## Self-Service Enrollment Phases

The self-service enrollment process can generally be divided into three phases:

| Phase | Purpose | Available Options / Notes |
| --- | --- | --- |
| **Phase 1 - Identify the User** | Users provide information so the system can uniquely identify them. | User ID / UPN, Employee ID, Phone Number, First Name + Last Name, Additional Attributes, or a combination of these. The selected combination should uniquely identify a user and should be based on information users commonly know. |
| **Phase 2 - Verify the User** | After identification, the system verifies that the matched user is correct. | Email verification code, SMS verification code, Automatic verification using Code/API, Self-confirmation, or None. |
| **Phase 3 - Configure PIN** | Users configure their PIN if required by the security key policy. | PIN setup is controlled by the configured security key policy. If required, users will be prompted to create and confirm their PIN during enrollment. |

## Verification Options

Below is a summary of the verification options in Phase 2:

| Verification Option | Description | Example Use Case |
| --- | --- | --- |
| **Email Code** | Sends a verification code to the user's registered email address. | Useful when email is a trusted and accessible channel for users. |
| **SMS Code** | Sends a verification code to the user's registered phone number. | Useful when mobile phone numbers are available and trusted. |
| **Automatic using Code / API** | Uses an existing system or association table to verify the user automatically. | For example, if a PACS, badge system, HR system, or internal database already maps users to badge numbers, that data can be validated through API without migrating it into IDmelon. |
| **Self-confirmation** | Shows the matched user information and asks the user to confirm it is correct. | Useful to prevent mistakes when users enter Employee ID, phone number, or other identifiers. |
| **None** | Skips the verification step after the user is identified. | Useful when the customer decides identification alone is sufficient for the enrollment flow. |
