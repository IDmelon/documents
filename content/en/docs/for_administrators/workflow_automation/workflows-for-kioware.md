---
title: "Configure workflows for KioWare"
description: ""
lead: ""
date: 2024-05-08T10:30:55+03:30
lastmod: 2024-08-12T10:30:55+03:30
draft: false
images: []
menu:
  docs:
    parent: "workflow_automation"
weight: 96300
toc: true
---

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
