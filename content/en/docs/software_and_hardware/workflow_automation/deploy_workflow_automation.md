---
title: "Deploying Workflow Automation"
description: ""
lead: ""
date: 2026-06-17T11:07:06+03:30
lastmod: 2026-06-17T11:07:06+03:30
draft: false
images: []
type: docs
weight: 321130
---

This guide explains how to deploy a workflow to multiple devices at scale. Once a workflow is created and tested in the `Workflow Editor`, you can roll it out across many machines using **Microsoft Intune**, **PowerShell Remoting**, or **PsExec**.

Deployment always involves two things on each target device:

1. **The exported workflow file** — a `.json` workflow exported from the Workflow Editor (see [Export a Workflow](../workflow_automation/#export-a-workflow)).
2. **The automation command** — the Accesskey command that registers the workflow to run on the security key presence (card tap) trigger (see [Configuring Accesskey](../workflow_automation/#configuring-accesskey)).

> **Prerequisite:** Workflow Automation (which includes the Workflow Runner) and IDmelon Accesskey (**version 3.12.1 or later**) must be installed on every target device. With Intune you can deploy these apps the same way you deploy any other application; see [Automatic Software Deployment](../../../for_administrators/automatic_software_deployment/deploy_with_intune/) for guidance on packaging and assigning apps.

<!-- -->

> **Note:** The scripts in this guide are examples and will need to be adjusted to your environment. Update them based on:
>
> - The **file name** and **path** of the exported workflow (`documentation_sample.json` is only a placeholder).
> - Whether the workflow `.json` file **already exists** on the device and you only want to update its contents, versus copying it for the first time.
> - Whether you want to **change the file name or path** referenced in the Accesskey automation command (`--workflow-path`), in which case re-register the command so it points to the new location.
> - The source location you copy the workflow from.

## Deploying in a Kiosk Environment

In a kiosk environment, deployment is the most important part of the rollout, because the `Workflow Editor` itself cannot run in kiosk mode (Windows allows only one application at a time in kiosk mode; see [Workflow Automation in Kiosk Environment](../configuring_for_kiosk_env/)).

The recommended approach is:

1. **Train the workflow on a normal user.** Build and test the workflow in a standard Windows account on a representative device, then export it as a `.json` file.
2. **Use Intune to set the automation command on every kiosk device.** Distribute the exported workflow file and register the automation command across all kiosk machines with an Intune platform script.

### Step 1: Train and export the workflow on a normal user

1. On a standard Windows account (not a kiosk account), open the `Workflow Editor` and create your workflow.
2. Make sure each UI element selector is configured so it is **not dependent on your local system**, so the workflow runs reliably on other devices (see [UI element selector](../workflow_automation/#ui-element-selector)).
3. Test the workflow end to end with the **Run** button.
4. Export the workflow as a `.json` file (see [Export a Workflow](../workflow_automation/#export-a-workflow)).

![Workflow Exportation](/images/vendor/workflow_automation/automation_app/workflow_exportation.png)
> **Figure:** Exporting a workflow.

### Step 2: Distribute the workflow and register the automation command via Intune

You will use a PowerShell **platform script** in Intune to copy the workflow file to a known location on each device and then register the automation command.

1. Place the exported workflow file where the script can reach it on the device (for example, package it alongside the script or copy it from a network share). In the example below, the workflow is expected at `C:\ProgramData\IDmelon\Workflow Automation\Workflows\documentation_sample.json`.

2. Create the deployment script and save it as a `.ps1` file:

   ```bash
   # Destination folder on each kiosk device
   $workflowDir = "C:\ProgramData\IDmelon\Workflow Automation\Workflows"
   $workflowPath = Join-Path $workflowDir "documentation_sample.json"

   # Ensure the workflow file is present (copy it from the packaged location or a share)
   if (-not (Test-Path $workflowDir)) {
       New-Item -Path $workflowDir -ItemType Directory -Force | Out-Null
   }
   # Example: copy the workflow shipped next to this script
   Copy-Item -Path "$PSScriptRoot\documentation_sample.json" -Destination $workflowPath -Force

   # Register the automation command so the workflow runs on the card-tap trigger
   accesskeycli workflow-automation -s -t automation-app --action execute --workflow-path "$workflowPath"
   ```

3. Create a PowerShell script policy in Intune:
   - Navigate: **Devices > Windows > Scripts and remediations > Platform Scripts**.
   - Select **Add**.
   - Basics: Name the script (e.g., "Deploy Workflow Automation").
   - Select **Next**.
   - Upload script: Select your `.ps1` file.
   - Run this script using the logged-on credentials: **No** (run as System).
   - Enforce script signature check: **No**.
   - Run script in 64-bit PowerShell: **Yes** (recommended on Windows 11).
   - Assignments: Assign to the kiosk **device** groups.
   - Review + add: Confirm and create the script deployment.

> **Note:** Run the script as **System** so the automation command is registered on the device regardless of which kiosk user logs on.

![Register Automation Command Intune - 1](/images/vendor/workflow_automation/automation_app/intune_platform_script_1.png)
![Register Automation Command Intune - 2](/images/vendor/workflow_automation/automation_app/intune_platform_script_2.png)
![Register Automation Command Intune - 3](/images/vendor/workflow_automation/automation_app/intune_platform_script_3.png)
> **Figure:** Powershell script policy in intune.

To remove the automation later, deploy a script that disables it:

```bash
accesskeycli workflow-automation -t automation-app -r
```

> **Tip:** You can also run the workflow through the Workflow Runner CLI from an Accesskey script instead of registering the automation app directly. See [Using the Workflow Runner CLI in the Accesskey script](../workflow_automation/#using-the-workflow-runner-cli-in-the-accesskey-script).

## Deploying with PowerShell Remoting

PowerShell Remoting lets you push the workflow and register the automation command on remote devices over WinRM. This is useful for ad-hoc rollouts or for environments without an MDM.

The Workflow Automation app itself can be installed either **from Intune** (see the prerequisite above) or **over the same remoting session** by copying the installer and running it silently, as shown in the optional step below.

> **Prerequisite:** PowerShell Remoting must be enabled on the target devices (`Enable-PSRemoting -Force`), and you need administrative credentials and network connectivity to the targets.

1. Connect to the target device.

   When the target is reached **by IP address or is not domain-joined**, add it to `TrustedHosts` and authenticate explicitly:

   ```bash
   $ComputerName = "192.168.1.80"

   # Required for IP / workgroup (non-domain) targets
   Set-Item WSMan:\localhost\Client\TrustedHosts -Value $ComputerName

   $Session = New-PSSession -ComputerName $ComputerName -Credential (Get-Credential) -Authentication Negotiate
   ```

   > For domain-joined devices reachable by host name, `New-PSSession -ComputerName $ComputerName` is usually enough (Kerberos handles authentication), and the `TrustedHosts` step is not needed.

2. (Optional) Install the Workflow Automation app over the session.

   Skip this step if the app is already deployed through Intune.

   ```bash
   $SetupSource = "C:\Users\you\Downloads\Workflow Automation\Setup.exe"
   $SetupDest   = "C:\Temp\Setup.exe"

   Invoke-Command -Session $Session -ScriptBlock { New-Item -Path "C:\Temp" -ItemType Directory -Force | Out-Null }
   Copy-Item -Path $SetupSource -Destination $SetupDest -ToSession $Session

   # Install silently and wait for it to finish
   Invoke-Command -Session $Session -ScriptBlock { Start-Process $using:SetupDest -ArgumentList "/S" -Wait }
   ```

   ![Installing Workflow Automation - client](/images/vendor/workflow_automation/automation_app/psr_installation_client.png)
   > **Figure:** Installing workflow automation using powershell remoting - client side.

   ![Installing Workflow Automation - 1](/images/vendor/workflow_automation/automation_app/psr_installation_1.png)
   ![Installing Workflow Automation - 2](/images/vendor/workflow_automation/automation_app/psr_installation_2.png)
   ![Installing Workflow Automation - 3](/images/vendor/workflow_automation/automation_app/psr_installation_3.png)
   > **Figure:** Installing workflow automation using powershell remoting - server side.

3. Copy the workflow file and register the automation command:

   ```bash
   $WorkflowSource = "C:\ProgramData\IDmelon\Workflow Automation\Workflows\documentation_sample.json"
   $WorkflowDest   = "C:\ProgramData\IDmelon\Workflow Automation\Workflows\documentation_sample.json"

   # Ensure the destination folder exists on the remote device, then copy the workflow file
   Invoke-Command -Session $Session -ScriptBlock { New-Item -Path "C:\ProgramData\IDmelon\Workflow Automation\Workflows" -ItemType Directory -Force | Out-Null }
   Copy-Item -Path $WorkflowSource -Destination $WorkflowDest -ToSession $Session

   Invoke-Command -Session $Session -ScriptBlock {
       accesskeycli workflow-automation -s -t automation-app --action execute --workflow-path $using:WorkflowDest
   }

   Remove-PSSession $Session
   ```

> **Note:** Inside `Invoke-Command -ScriptBlock { ... }`, local variables are not available on the remote device. Use the `$using:` prefix (for example, `$using:WorkflowDest`) to pass a local value into the remote session.

![Register Automation Command PSR - server](/images/vendor/workflow_automation/automation_app/PRS_server.png)
> **Figure:** Registering automation command - server side.

![Register Automation Command PSR - client](/images/vendor/workflow_automation/automation_app/PRS_result_client.png)
   > **Figure:** Registering automation command - result on client side.

## Verifying the Deployment

After deployment, confirm that the automation runs on each device:

1. Tap a registered badge on the reader on a target device.
2. The Workflow Runner should execute the deployed workflow automatically (see [Test Workflow](../workflow_automation/#test-workflow)).

> **Note:** If the workflow does not run, verify that Workflow Automation and Accesskey are installed, that the workflow file exists at the path used in the automation command, and that the UI element selectors are not dependent on the machine where the workflow was trained.

For troubleshooting, check the Workflow Automation log files on the target device:

```shell
C:\ProgramData\IDmelon\Workflow Automation\Logs
```
