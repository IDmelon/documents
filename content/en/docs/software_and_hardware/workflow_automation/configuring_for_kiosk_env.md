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

## Workflow Runner in Kiosk Mode

To execute workflows, the **Workflow Runner** must run in the background. As noted earlier, kiosk mode restricts the number of applications that can run simultaneously. To bypass this limitation, follow the steps below:

### Multi-app kiosk mode

If you are using **Microsoft Intune** as an MDM provider, do the following steps:

1. Go to [Intune Portal](https://intune.microsoft.com).
2. Navigate to **Devices > Windows > Configuration**, and select the kiosk configuration you previously created.
3. In the **Configuration settings** section, select **Edit**.
4. Under **Browsers and Applications**, select **Add Win32 app**.
5. Complete the following fields:
   - **Application name**: IDmelon Workflow Runner
   - **Local path to app executable file**: C:\Program Files\IDmelon\Workflow Automation\WorkflowRunner.exe
   - **Application user model ID**: com.idmelon.workflowrunner
6. Click **OK**.
7. Uncheck the **Autolaunch**.
8. Save the configuration.

### Single-app kiosk mode

If your kiosk is configured in single-app full-screen mode, you should configure the **IDmelon Accesskey** to launch the Workflow Runner app when the user logs in.

To configure, do the following steps:

1. Make a new folder named `Startup` in the following path if it does not already exist:

   ```bash
   C:\Program Files (x86)\IDmelon\Accesskey 
   ```

2. Place a shortcut of `WorkflowRunner.exe` (located in `C:\Program Files\IDmelon\Workflow Automation`) in the `Startup` folder.

> If your kiosk environment is configured using Assigned Access (see [Configure a single-app kiosk with Assigned Access](https://learn.microsoft.com/en-us/windows/configuration/assigned-access/configure-single-app-kiosk?tabs=ps)), you can add the Workflow Automation path as an allowed path in AppLocker to ensure the Workflow Runner app is not blocked from running.
