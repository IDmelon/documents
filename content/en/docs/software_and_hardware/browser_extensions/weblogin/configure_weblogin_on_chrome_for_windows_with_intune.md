---
title: "Configure on Chrome for Windows using Intune"
description: ""
lead: ""
date: 2025-12-30T14:40:56+01:00
lastmod: 2025-12-30T14:40:56+01:00
draft: false
images: [ ]
menu:
    docs:
        parent: "weblogin"
type: docs
weight: 326000
toc: true
---

This guide provides a PowerShell script for IT Administrators to configure the WebLogin extension on Google Chrome via
Microsoft Intune. This script manages complex configuration objects—including nested server addresses, automation
workflows, and operational options—by writing directly to the Windows Registry.

## Prerequisites

- Intune Administrator permissions.

- Windows 10/11 devices enrolled in Intune.

- The WebLogin extension must be
  force-installed ([Guide](/docs/software_and_hardware/browser_extensions/weblogin/install_weblogin_on_chrome_for_windows_with_intune/)).

## Step 1: Create the PowerShell Script

Create a script named `Configure-WebLogin-Chrome.ps1`. Update the variables in the script to match your organization's
environment. Please note all the configs are optional and you should only declare the needed configs.

```shell
# Declare needed configs

$api_key = "your-api-key-goes-here"
$workspace_id = "your-workspace-id-goes-here"
$server_address = "{ `"env`": `"onpremise`", `"env`": `"https://domain.com/api/url`" }"
$options = "{ `"handlePasskeyRequests`": { `"value`": true }, `"allowAddingNewPasswords`": { `"value`": true }, `"allowPasswordUpdatePrompts`": { `"value`": true }, `"useEmbeddedNumpad`": { `"value`": true }, `"keystroking`": { `"value`": true }, `"debugMode`": { `"value`": true }, `"autofillPrompt`": { `"value`": true, `"includedUrls`": [`"https://domain.com/login`"], `"excludedUrls`": [`"https://domain.com/login`"] } }"
$workflow_automation = "{ `"action`": `"login`", `"window`": `"incognito`", `"hint`": { `"type`": `"pinTapPage`" }, `"urls`": [ { `"method`": `"passkey`", `"url`": `"https://myapps.microsoft.com`" } ] }"

$registry_path = "HKLM:\SOFTWARE\Policies\Google\Chrome\3rdparty\extensions\eagmgpbjpedchliifpgfgogdknnmkaej\policy"

$parent_path = Split-Path -Path $registry_path

# Create policy path for the WebLogin if it does not exist

if (!(Test-Path $parent_path)) {
    New-Item -Path $parent_path -Force -ItemType Directory | Out-Null
}

if (!(Test-Path $parent_path\policy)) {
    New-Item -Path ("$parent_path\policy") -Force -ItemType Directory | Out-Null
}

# Set values

Set-ItemProperty -Path $registry_path -Name "apiKey" -Value $api_key
Set-ItemProperty -Path $registry_path -Name "workspaceID" -Value $workspace_id
Set-ItemProperty -Path $registry_path -Name "serverAddress" -Value $server_address -Type ExpandString
Set-ItemProperty -Path $registry_path -Name "options" -Value $options -Type ExpandString
Set-ItemProperty -Path $registry_path -Name "workflowAutomation" -Value $workflow_automation -Type ExpandString

exit 0
```

## Step 2: Deployment via Intune

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com/).

2. Navigate to **Devices > Scripts and remediations > Platform scripts**.

3. Click **Add**.

   ![alt](/images/vendor/weblogin/create_script.jpg)

4. **Basics:** Name the script (e.g., Chrome - WebLogin Extension Managed Config).

5. **Script Settings**:

    - **Script location:** Upload `Configure-WebLogin-Chrome.ps1`.

    - **Run this script using the logged on credentials:** No.

    - **Enforce script signature check:** No.

    - **Run script in 64-bit PowerShell Host:** No.

   ![alt](/images/vendor/weblogin/script_settings_chrome.jpg)

6. **Assignments:** Assign to the device group that has the WebLogin extension force-installed.

## Step 3: Verification

To confirm the settings have been applied correctly:

1. **Check the Registry:** Open `regedit` and navigate to:
   `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\3rdparty\extensions\eagmgpbjpedchliifpgfgogdknnmkaej\policy`
   Verify that all declared keys (`apiKey`, `serverAddress`, etc.) are present.

2. **Verify in Chrome:** Open Chrome and navigate to `chrome://policy`. Find the entry for
   `eagmgpbjpedchliifpgfgogdknnmkaej`. Under Policy Value, click Show More to see the structured JSON data being
   consumed by the extension.

> Intune can take up to 24 hours to run your configuration script on the end device.
