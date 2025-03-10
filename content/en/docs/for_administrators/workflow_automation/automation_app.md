---
title: "Automation App"
description: ""
lead: ""
date: 2024-05-08T10:30:55+03:30
lastmod: 2024-08-12T10:30:55+03:30
draft: false
images: []
menu:
  docs:
    parent: "workflow_automation"
weight: 96100
toc: true
---

The Workflow Automation application is an extension that can be installed on PC along with IDmelon Accesskey to automate the user login and logout routine in kiosk environments (Windows Kiosk, KioWare, etc.). Therefore, the login (and logout) steps are done once by a system admin, and then by tapping the user's card on the reader, the login steps are done automatically, as well as for the user to exit the kiosk environment.

## Prerequisites

- [Card Reader Drivers](https://idmelon.com/docs/downloads) (3.5.2 and later)
- [Workflow Automation App](https://idmelon.com/docs/downloads)

## Configuring a workflow

To switch to automation mode, enter the following command in PowerShell.

```shell
accesskeycli workflow-automation -s -t automation-app -a login-logout -e kiosk-interactive
```

- `-s`: To activate automation and configure it.
- `-t automation-app`: This value is always fixed.
- `-a login-logout`: To set the automation actions. In this case, both the login and logout procedures can be automated. (Other possible actions: `-a login` , `-a logout`)
- `-e kiosk-interactive`: To set the environment. In this case, we want to use the kiosk environment of Windows 10 and 11. (Other possible environments: `kioware`, `kiosk-multiapps`, `-e kiosk-browser`).
- `-p microsoft-login` (Optional): To set the target platform. In this case, our goal is Microsoft 365 login and logout automation.
- `-u https://myapps.microsoft.com?login_hint=${UserId}` (Optional): To set the target URL. In this case, we set the Microsoft 365 URL.

**App Configuration File:**

The configuration file of the Workflow app is in the following path, which you can customize.

```shell
C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation\configs.json
```

- **unregistered_card_title**: The title of the unregistered card error alert.
- **unregistered_card_message**: The message of the unregistered card error alert.
- **delete_linked_devices_on_start**: Delete the linked device list (A list of the smartphones that are displayed on the Windows Security prompt) on system startup.
- **delete_linked_devices_on_each_tap**: Delete the linked device list on each card tap.
- **template_matching_confidence**: The level of accuracy (a number between 0 and 1) in matching the templates for the points that should be clicked.
- **logout_on_tap_if_not_on_the_first_page** *(app version 1.2.1 and later)*: If the value is set to **true**, the user will log out every time the card taps on the reader, except on the first (login) page.
- **logout_method** *(app version 1.2.1 and later)*: The program uses two methods(default is **0**) to close the kiosk window. If this does not happen correctly on some systems, set its value to **1**.
- **support_email** *(app version 1.3.0 and later)*: The default email address to which reports are sent when an issue occurs within the app.

**Deploy App Configuration File:**

To deploy the configuration, copy the file **configs.json** in the path **C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation** and deploy it to the desired systems in the same path.

### Kiosk

For **kiosk single app** (full-Screen kiosk), enter the following command in PowerShell:

```shell
accesskeycli workflow-automation -s -t automation-app -e kiosk-interactive
```

For **multi app kiosk**, enter the following command in PowerShell:

```shell
accesskeycli workflow-automation -s -t automation-app -e kiosk-multiapps
```

### KioWare

One of the software that has implemented the kiosk environment is KioWare, which is supported by the IDmelon Workflow Automation extension.
The only difference between KioWare and the Windows kiosk is the need to train the logout operation.

- Open `KioWare Config Tool` on your system.

- Select the option **Attract/Inactivity** from the left menu.

- From the **Session End Settings** section, set the **When the session ends, clear:** option as follows:

![Automation Train](/images/vendor/workflow_automation/automation_app/kioware_menu1.png)

- Select the option **General** from the left menu.

- From the **Exit Passcodes** section, click on **Add Exit Passcode** button.

![Automation Train](/images/vendor/workflow_automation/automation_app/kioware_menu2.png)

- From the new dialog that opens, set the **Exit Action** option to `Restart KioWare`.

- In the **Change Passcode** section, enter your desired passcode.

- Click on **Done** button.

![Automation Train](/images/vendor/workflow_automation/automation_app/kioware_menu3.png)

## Train a workflow

The training operation is performed only once by the admin.<br>
If no workflow is predefined for the target platform while tapping the card, the training hints window will be displayed and the training procedure will begin.

- Navigate to the website or application you need training for.
- Tap a card on the reader.
- Click the **Yes** button on the presented alert if you want to start the training.

![Automation Train](/images/vendor/workflow_automation/automation_app/no_workflow_found.png)

- The training hints window will appear as follows.

![Automation Train](/images/vendor/workflow_automation/automation_app/training_hints_window.png)

- Leave the toggle of the **Display Tap to Login Page** on to see the **Tap Your Card** alert on the top of the start page later for each login.

![Automation Train](/images/vendor/workflow_automation/automation_app/tap_card_alert.png)

- Toggle the **Enable Error Message for Unregistered Cards** on to display an error after tapping a card which is not enrolled yet.

![Automation Train](/images/vendor/workflow_automation/automation_app/card_unregistered_alert.png)

- Click the **Start** button to start the training.
- The recording icon will appear in the bottom right corner of the screen which means that each click will be captured as a step.

![Automation Train](/images/vendor/workflow_automation/automation_app/recording_icon.png)

- During the recording process, try to click only on the icons, buttons, or links, and not on the white spaces. After all the clicks are captured as all the necessary steps, click on the recording icon to stop training.

*Note: Currently, for the kiosk environment (kiosk-interactive, kiosk-browser, kiosk-multiapps), the logout operation does not need to be trained, and when the user's card is tapped to log out, the kiosk environment is automatically restarted.*

## Deploying a pre-trained workflow

The workflow setup can be trained once on a single kiosk and then deployed to other kiosks with the same hardware configuration, including display settings. This means that the training steps are completed only once by an administrator, and the workflow files can then be copied to other kiosks without requiring separate training on each individual kiosk.

*Note: The **Monitor Dimensions**, **Scale** and **Display Resolution** of all systems must be the same.*

![Automation Train](/images/vendor/workflow_automation/automation_app/screen_resolution.png)

To deploy the workflow files to additional kiosks, follow these steps:

1. **Copy the Workflow Folder**<br>
   Copy the following folder from the trained Kiosk/PC:

   ```bash
   C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation\Actions
   ```

2. **Paste the Folder on Target PCs**  
   On each target PC, paste the folder into a path such as:

   ```bash
   C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation\Actions
   ```

   You can use the PowerShell code below to automatically copy the files for example:

   ```powershell
   Copy-Item -Path "C:\kiosk\Actions" -Destination "C:\Program Files (x86)\IDmelon\Accesskey\Extensions\WorkflowAutomation\Actions" -Recurse -Force
   ```

This process allows the trained workflow to be used across kiosks with the same hardware configuration, eliminating the need for retraining on each device.

## Automation

After the training process is over, everything is ready to use.
When the user taps the card on the reader, if the login page is open in the kiosk environment, the automation will be started and clicks will be done automatically.
If the user has already logged in and the card is tapped again, the kiosk will automatically restart and be ready for the next user to log in.

## Disabling a workflow

To disable training prompt, enter the following command in PowerShell.

```shell
accesskeycli workflow-automation -c disable-auto-capture
```

To disable the Workflow Automation, enter the following command in PowerShell.

```shell
accesskeycli workflow-automation -r -t automation-app
```

## Report Issue

If the user's badge is not registered in the system, or if the number of incorrect PIN attempts has been reached, or if the automation is not performed correctly, the user will be prompted to report the issue. By clicking the **Send Report** button, the report will be sent to the support email along with the application logs and user information.<br>
***Note:** To change the default support email, see the **[Configuring a workflow](#configuring-a-workflow) > App Configuration File** section.*

![Report issue not enrolled](/images/vendor/workflow_automation/automation_app/report_issue_not_enrolled.png)
![Report issue PIN locked](/images/vendor/workflow_automation/automation_app/report_issue_pin_locked.png)
