---
title: "Workflow Automation in Kiosk Environment"
description: ""
lead: ""
date: 2025-12-30T11:07:06+03:30
lastmod: 2025-30-14T11:07:06+03:30
draft: false
images: []
type: docs
weight: 321120
---

Due to Windows limitations in kiosk environments, only one application can run at a time. This means the IDmelon Workflow Editor cannot be executed directly in kiosk mode.

The recommended solution is to create the workflow in a standard Windows account. For example, if Microsoft Edge is configured as the kiosk application, you can open Edge in a normal Windows account, manage your workflow there, and then export it for use in kiosk mode.

> To ensure that the workflow created in the normal account functions properly in kiosk mode, verify that the selector for each UI element is correctly configured (see [UI element selector](../workflow_automation/#ui-element-selector)).
