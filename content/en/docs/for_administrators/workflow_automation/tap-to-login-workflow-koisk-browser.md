---
title: "Tap to Login for Kiosk Browser"
description: ""
lead: ""
date: 2024-05-08T10:30:55+03:30
lastmod: 2025-03-10T10:30:55+03:30
draft: false
images: []
menu:
  docs:
    parent: "workflow_automation"
weight: 96100
toc: true
---

This guide explains how to configure a tap-to-login workflow in a kiosk environment using Microsoft Edge as the native browser. When a user taps their RFID/NFC badge the first time, the system automatically initiates the login process. A second tap can then be configured to trigger a secure logout. This approach streamlines kiosk access, boosting productibity.

## Prerequisites

- [Card Reader Drivers](https://idmelon.com/docs/downloads)
- [Workflow Automation App](https://idmelon.com/docs/downloads)

## Configuring tap-to-login workflow

To enable tap-to-login and tap-to-logout actions with a badge for Microsoft, open PowerShell and run the following command to configure the Accesskey Reader Driver:

```shell
accesskeycli workflow-automation -s -t automation-app -a login-logout -e kiosk-interactive
```

- `-s`: To set a workflow.
- `-t automation-app`: Targetting Automation App.
- `-a login-logout`: Other supported actions are `-a login` , `-a logout`)
- `-e kiosk-interactive`: Other supported environemts are : `kioware`, `kiosk-multiapps`, `-e kiosk-browser`).

## Configure the settings (Optional)

You can customize the Workflow app by modifying its configuration file, located at:

```shell
C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation\configs.json
```

Within this file, you can adjust error messages, define login/logout behavior, and more. Below are the key parameters:

- **unregistered_card_title**: The title of the unregistered card error alert.
- **unregistered_card_message**: The message of the unregistered card error alert.
- **delete_linked_devices_on_start**: Delete the linked device list (A list of the smartphones that are displayed on the Windows Security prompt) on system startup.
- **delete_linked_devices_on_each_tap**: Delete the linked device list on each card tap.
- **template_matching_confidence**: The level of accuracy (a number between 0 and 1) in matching the templates for the points that should be clicked.
- **logout_on_tap_if_not_on_the_first_page** *(app version 1.2.1 and later)*: If the value is set to **true**, the user will log out every time the card taps on the reader, except on the first (login) page.
- **logout_method** *(app version 1.2.1 and later)*: The program uses two methods(default is **0**) to close the kiosk window. If this does not happen correctly on some systems, set its value to **1**.
- **support_email** *(app version 1.3.0 and later)*: The default email address to which reports are sent when an issue occurs within the app.

## Train the login flow

The training process is typically done only once by an administrator. When the system has no existing training data (i.e., the Actions folder has not been created), the first badge tap will prompt you to create a login flow training sequence. After training, you can transfer the generated Actions folder to other PCs that share the same screen resolution for consistent behavior.

To train a login flow, simply navigate to the login screen and tap the badge on the reader. The workflow automation tool will be ready to capture and record the required steps.

![Automation Train](/images/vendor/workflow_automation/automation_app/no_workflow_found.png)

Click Yes to begin the training sequence and follow the on-screen hints.
![Automation Train](/images/vendor/workflow_automation/automation_app/training_hints_window.png)

- **Display Tap to Login Page**: Leave this toggle On if you want the “Tap Your Card” prompt to appear at the top of the login page for each sign-in.
![Automation Train](/images/vendor/workflow_automation/automation_app/tap_card_alert.png)
- **Enable Error Message for Unregistered Cards**: Turn this toggle On if you wish to display an error message whenever a user taps a badge that is not enrolled. See [Send Report](#report-issue).

Click Start to initiate the recording. A recording icon will appear at the bottom right corner of your screen, indicating that each click is being captured as a step. For better accuracy, focus your clicks on clearly visible icons and buttons.

![Automation Train](/images/vendor/workflow_automation/automation_app/recording_icon.png)

After performing all necessary actions, click the recording icon again to stop the training process.

*Note: Currently, for the kiosk environment (kiosk-interactive, kiosk-browser, kiosk-multiapps), the logout operation does not need to be trained, and when the user's card is tapped to log out, the kiosk environment is automatically restarted.*

## Deploying a pre-trained workflow to other Kiosks

When training is done on one kiosk then it can be deployed to other kiosks with the same hardware configuration, including display settings. This means that the training steps are completed only once by an administrator, and the workflow files can then be copied to other kiosks without requiring separate training on each individual kiosk.

To deploy the workflow files to additional kiosks, follow these steps:

1. **Copy the Workflow Folder**<br>
   Copy the following `config.json` file and `Action` folder from the following path:

   ```bash
   C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation\Actions
   C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation\configs.json
   ```

2. **Paste the Folder on Target PCs**  
   On each target PC, paste the `config.json` file and `Action` directory into a path such as:

   ```bash
   C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation\
   ```

## Test & Troubleshooting

Once training is complete, the system is ready to use. Here’s how it behaves:

- **Automatic Login**: If the kiosk is on the login screen when a user taps their card, the recorded automation steps will run immediately, logging the user in without any manual clicks.
- **Automatic Logout**: If someone is already logged in, tapping a card again restarts the kiosk session—readying it for the next user to log in.

*Note that the Monitor Dimensions, Scale and Display Resolution of all systems must be the same.*

![Automation Train](/images/vendor/workflow_automation/automation_app/screen_resolution.png)

### Report Issue

If the user's badge is not registered in the system, or if the number of incorrect PIN attempts has been reached, or if the automation is not performed correctly, the user will be prompted to report the issue. By clicking the **Send Report** button, the report will be sent to the support email along with the application logs and user information.<br>
***Note:** To change the default support email, see the **[optional settings for configs.json](#configure-the-settings-optional).**

![Report issue not enrolled](/images/vendor/workflow_automation/automation_app/report_issue_not_enrolled.png)
![Report issue PIN locked](/images/vendor/workflow_automation/automation_app/report_issue_pin_locked.png)
