---
title: "Configuring workflows with scripts"
description: ""
lead: ""
date: 2024-07-17T10:30:55+03:30
lastmod: 2024-07-17T10:30:55+03:30
draft: false
images: [ ]
menu:
  docs:
    parent: "workflow_automation"
weight: 96200
toc: true
---

## Overview

IDmelon Reader/Scanner Driver provides the script based access automation workflows which help users run a Windows Powershell Script on some defined conditions.

When such a workflow is configured in IDmelon Reader/Scanner Driver, on the defined trigger condition the script is executed by the IDmelon Driver Service.

## What can scripts do?

With the Windows powershell scripts, most of the operating system services and applications are available to execute and control the OS level actions. But with IDmelon Reader/Scanner Driver some additional features are provided for users to perform a web login, a mouse click workflow automation, etc. These features are provided as CLI commands which can be executed from the powershell script.

## How to configure the IDmelon Reader/Scanner Driver

In order to configure the IDmelon Driver to execute a script on a condition, all you need to do is creating a script file, putting it somewhere in the PC (Inside the system driver), and enter the command bellow:

```shell
accesskey config access-automation set --trigger-condition onSecurityKeyPresence --script-path "PATH_TO_SCRIPT_FILE"
```

Available trigger conditions are: onSecurityKeyPresence, onLogon, onUnlock

## OS Actions

### Opening an application and putting it on foreground

Here is a sample script which opens the Google Chrome:

```shell
Add-Type @"
using System;
using System.Runtime.InteropServices;

public class User32 {
    [DllImport("user32.dll")]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool SetForegroundWindow(IntPtr hWnd);

    [DllImport("user32.dll")]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool ShowWindowAsync(IntPtr hWnd, int nCmdShow);
}
"@

function BringChromeToFront {
    $chromeProcess = Get-Process -Name "chrome" -ErrorAction SilentlyContinue

    if ($chromeProcess) {
        foreach ($process in $chromeProcess) {
            $hwnd = $process.MainWindowHandle
            if ($hwnd -ne [IntPtr]::Zero) {
                # SW_MAXIMIZE = 3
                [User32]::ShowWindowAsync($hwnd, 3) | Out-Null
                [User32]::SetForegroundWindow($hwnd) | Out-Null
                break
            }
        }
    } else {
        Write-Host "Chrome is not running. Starting Chrome..."
        Start-Process "chrome"
    }
}
```

A known issue: Microsoft Edge can not be opened using this command in an IDmelon Script Based Access Automation.

### Closing an application

Here is a sample script which closes all instances of Google Chrome:

```shell
function Close-AllChromeInstances {
    $chromeProcesses = Get-Process -Name "chrome" -ErrorAction SilentlyContinue

    if ($chromeProcesses) {
        foreach ($process in $chromeProcesses) {
            Write-Host "Closing Chrome process with ID: $($process.Id)"
            $process.Kill()
        }
        Write-Host "All Chrome instances have been closed."
    } else {
        Write-Host "No Chrome processes found."
    }
}
```

### Locking the screen

```shell
rundll32.exe user32.dll,LockWorkStation
```

### Signing the user out

```shell
logoff
```

## IDmelon Driver Actions

There are two different actions available in IDmelon Driver. Some of the actions are executable and some return a specific data or status. All of the available commands are explained bellow with a complete script sample which can be used in real world.

### IDmelon Driver Response Format

The response from the IDmelon Driver is always a JSON string which always contains a 'result' key which defines whether the command is executed successfully.
If the result value is 0, it means the command is executed successfully.

If the result value is a positive number, it means the command is failed and there had been an exception or error inside the IDmelon Driver Service.

If the result value is a negative number, it means the command is failed and there had been an exception or error in the third-party app/service which had to execute all or part of the command.

When the result is NOT 0, the error message can be found in the response JSON with key name 'error'.

```json
{
  "result": 0,
  ...
}
```

```json
{
  "result": 1,
  "error": "Something went wrong!"
}
```

### Get the security key info

```shell
accesskey access-automation get security-key-info
```

The result JSON:

```json
{
  "result": 0,
  "lastPresentedSecurityKeyInfo": {
    "id": "04115D4A774680",
    "type": 2,
    "pinStatus": true,
    "userId": "arian@vancosys.com"
  },
  "lastUsedSecurityKeyInfo": {
    "id": "04115D4A774680",
    "type": 2,
    "pinStatus": true,
    "userId": "arian@vancosys.com",
    "matchesTheLastPresented": true
  },
  "lastSecurityKeyInfoUsedForWindowsLogin": {
    "id": "04115D4A774680",
    "type": 2,
    "pinStatus": true,
    "userId": "arian@vancosys.com",
    "matchesTheLastPresented": false
  }
}
```

### Get the screen status

```shell
accesskey access-automation get screen-status
```

The result JSON:

```json
{
  "result": 0,
  "isLocked": false
}
```

### Windows app password auto-fill

This command can be used to fill the card UID in an active text box any where in the system. Or it can be used for Windows Application authentication through auto password filling. Note that for the password filling to work, the desiring app must be focused before executing this command.

```shell
accesskey access-automation execute auto-fill --method [THE_METHOD_OF_AUTOFILL]
```

The supported methods of autofill:

1. default : Fills username, presses a tab then fills password
2. username: Only fills the username
3. password: Only fills the password
4. card-id: Fills the tapped card UID if the Card Reader Driver is installed

Add the `--press-enter-after-filling` if you wish to perform an `Enter` key press after filling.

Add the `--onboard` option if you want IDmelon to show a prompt for password onboarding in case the user has no password for the current focused app.

### Switch the logged-in user

This command can be used to switch the logged-in user.

```shell
accesskey access-automation execute switch-user
```

Add the `--sign-out-current-user` option if you wish to sigh out from the current user. Otherwise, IDmelon will only lock the screen.

### Enable the transparent lock

With this command a transparent screen covers the screen allowing the user to monitor the Windows but will not have access to make any changes.

```shell
accesskey access-automation execute transparent-lock
```

Users can login using security key or username-password.

### Get the last action of IDmelon Automation App

```shell
accesskey access-automation automation-app get status
```

The result JSON:

```json
{
  "result": 0,
  "lastAction": "none"
}
```

The last action can be 'none', `login`, `logout`.

### Get the last action of IDmelon WebLogin Extension

```shell
accesskey access-automation weblogin get status
```

The result JSON:

```json
{
  "result": 0,
  "lastAction": "none"
}
```

The last action can be `none`, `login`, `logout`.

### Get the status of IDmelon WebLogin Extension

This command is for checking if the WebLogin extension is available or not.

```shell
accesskey access-automation weblogin get status
```

The result JSON:

```json
{
  "result": 0,
  "webLoginExtensionIsAvailable": true
}
```

### Get the config of IDmelon WebLogin extension

This command is for checking the WebLogin extension is configured with any workflow automation.

```shell
accesskey access-automation weblogin get config
```

The result JSON:

```json
{
  "result": 0,
  "workflowAutomation": null
}
```

The 'workflowAutomation' can be null or a JSON which shows the parameters of the workflow automation.

### Automate user web login

```shell
accesskey access-automation weblogin execute automate --action "ACTION_TO_BE_PERFORMED" --window "THE_WINDOW_TO_OPEN_EXECUTE_THE_ACTION_IN" --url "THE_LOGIN_URL" --method "THE_METHOD_OF_LOGIN --timeout "TIMEOUT_IN_MILLISECONDS"
```

Available actions: login, logout, login-logout (The third action performs the login if the user is logged out and vice versa)

Available window options: current, newTab, incognito\

Available methods: password, passkey

The URL format: "https://myapps.microsoft.com?login_hint={UserId}"

Note that the timeout value is in milliseconds

This is a sample command for performing a web login with passkey. For more info about the options of this command.

```shell
accesskey access-automation weblogin execute automate --action "login" --window "newTab" --url "https://myapps.microsoft.com?login_hint={UserId}" --method passkey -t 120000
```

### Execute pre-trained login workflow (click simulation)

```shell
accesskey access-automation automation-app execute automate --action "ACTION_TO_BE_PERFORMED" --environment "THE_WORKING_ENVIRONMENT" --login-max-idle-time "TIME_IN_MINUTES" --timeout "TIMEOUT_IN_MILLISECONDS"
```

Available actions: login, logout, login-logout

Available environments: TBD

If you wish to perform a training inn case no training data is found, add the `---auto-capture` option.

### Start capturing for a pre-trained login workflow (click simulation)

```shell
accesskey access-automation automation-app execute capture --timeout "TIMEOUT_IN_MILLISECONDS"
```

## How to parse the IDmelon service action responses

As mentioned, the responses of the IDmelon service are in JSON string. Here are two sample scripts which together perform a user access automation on Windows logon/unlock and on security key presence.

### The script for OnLogon and OnUnlock trigger conditions

```shell
# Wait up to 60 seconds for Microsoft Edge to open a visible window, then
# maximize it and bring it to the foreground. Continues execution afterwards.

Add-Type @"
using System;
using System.Runtime.InteropServices;
public class Win32 {
    [DllImport("user32.dll")] public static extern bool SetForegroundWindow(IntPtr hWnd);
    [DllImport("user32.dll")] public static extern bool ShowWindowAsync(IntPtr hWnd, int nCmdShow);
    [DllImport("user32.dll")] public static extern bool IsIconic(IntPtr hWnd);
}
"@

# Win32 ShowWindow/ShowWindowAsync codes we use
$SW_RESTORE      = 9
$SW_SHOWMAXIMIZED = 3

$timeout = 60
$elapsed = 0

Write-Host "Waiting for Microsoft Edge to start and create a window..."

$edgeWindowProc = $null

# Wait until Edge has at least one process with a non-zero MainWindowHandle
while ($elapsed -lt $timeout) {
    $candidates = Get-Process -Name "msedge" -ErrorAction SilentlyContinue |
                  Where-Object { $_.MainWindowHandle -ne 0 -and $_.MainWindowTitle }

    if ($candidates) {
        # Prefer a window that looks like an Edge top-level window
        $edgeWindowProc = $candidates |
            Sort-Object StartTime -Descending |
            Select-Object -First 1
        break
    }

    Start-Sleep -Seconds 1
    $elapsed++
}

if (-not $edgeWindowProc) {
    Write-Warning "Timed out waiting for a visible Edge window after $timeout seconds."
    return
}

$hWnd = $edgeWindowProc.MainWindowHandle

Write-Host "Found Edge window (PID $($edgeWindowProc.Id)). Bringing to front and maximizing..."

# If minimized, restore first (prevents maximize from leaving it minimized)
if ([Win32]::IsIconic($hWnd)) {
    [Win32]::ShowWindowAsync($hWnd, $SW_RESTORE) | Out-Null
}

# Maximize
[Win32]::ShowWindowAsync($hWnd, $SW_SHOWMAXIMIZED) | Out-Null

# Bring to foreground/focus
[Win32]::SetForegroundWindow($hWnd) | Out-Null

Write-Host "Edge maximized and focused."

try {
    Write-Host "Checking the extension's status..."
    $response = & accesskey access-automation weblogin get status -t 10
    $response
    $jsonResponse = $response | ConvertFrom-Json
    if ($jsonResponse.PSObject.Properties["result"] -and 
        $jsonResponse.result -eq 0 -and $jsonResponse.PSObject.Properties["webLoginExtensionIsAvailable"] -and 
        $jsonResponse.webLoginExtensionIsAvailable -eq $true) {
        Write-Host "Getting the extension's last action..."
        $response = & accesskey access-automation weblogin get last-action
        $response
        $jsonResponse = $response | ConvertFrom-Json
        if ($jsonResponse.PSObject.Properties["result"] -and 
            $jsonResponse.result -eq 0 -and $jsonResponse.PSObject.Properties["lastAction"] -and 
            $jsonResponse.lastAction -eq "login") {
            Write-Host "Logging out the previous user..."
            $response = & accesskey  access-automation weblogin execute automate --action "logout" --window "incognito" --url "https://myapps.microsoft.com?login_hint={UserId}" --method passkey -t 5000
            $response
        }
        Write-Host "Logging in the user..."
        $response = & accesskey  access-automation weblogin execute automate --action "login" --window "incognito" --url "https://myapps.microsoft.com?login_hint={UserId}" --method passkey -t 120000
        $response
    }
} catch {
    Write-Host "Error running the script: $_"
}
```

This script waits for Microsoft Edge for 60 seconds then brings it to the foreground and maximizes it. Then it waits for the IDmelon WebLogin extension for 10 seconds and onces it is available, it checks the last action of the extension. If the last action is `login`, it sends a `logout` command to the extension and waits for its response for 5 seconds. Finally it sends the `login` command to the extension resulting the user being automatically logged in to the provided URL.

### The script for security key presence trigger condition

```shell
# Wait up to 60 seconds for Microsoft Edge to open a visible window, then
# maximize it and bring it to the foreground. Continues execution afterwards.

Add-Type @"
using System;
using System.Runtime.InteropServices;
public class Win32 {
    [DllImport("user32.dll")] public static extern bool SetForegroundWindow(IntPtr hWnd);
    [DllImport("user32.dll")] public static extern bool ShowWindowAsync(IntPtr hWnd, int nCmdShow);
    [DllImport("user32.dll")] public static extern bool IsIconic(IntPtr hWnd);
}
"@

# Win32 ShowWindow/ShowWindowAsync codes we use
$SW_RESTORE      = 9
$SW_SHOWMAXIMIZED = 3

$timeout = 60
$elapsed = 0

Write-Host "Waiting for Microsoft Edge to start and create a window..."

$edgeWindowProc = $null

# Wait until Edge has at least one process with a non-zero MainWindowHandle
while ($elapsed -lt $timeout) {
    $candidates = Get-Process -Name "msedge" -ErrorAction SilentlyContinue |
                  Where-Object { $_.MainWindowHandle -ne 0 -and $_.MainWindowTitle }

    if ($candidates) {
        # Prefer a window that looks like an Edge top-level window
        $edgeWindowProc = $candidates |
            Sort-Object StartTime -Descending |
            Select-Object -First 1
        break
    }

    Start-Sleep -Seconds 1
    $elapsed++
}

if (-not $edgeWindowProc) {
    Write-Warning "Timed out waiting for a visible Edge window after $timeout seconds."
    return
}

$hWnd = $edgeWindowProc.MainWindowHandle

Write-Host "Found Edge window (PID $($edgeWindowProc.Id)). Bringing to front and maximizing..."

# If minimized, restore first (prevents maximize from leaving it minimized)
if ([Win32]::IsIconic($hWnd)) {
    [Win32]::ShowWindowAsync($hWnd, $SW_RESTORE) | Out-Null
}

# Maximize
[Win32]::ShowWindowAsync($hWnd, $SW_SHOWMAXIMIZED) | Out-Null

# Bring to foreground/focus
[Win32]::SetForegroundWindow($hWnd) | Out-Null

Write-Host "Edge maximized and focused."

try {
    Write-Host "Checking the extension's status..."
    $response = & accesskey access-automation weblogin get status -t 10000
    $response
    $jsonResponse = $response | ConvertFrom-Json
    if ($jsonResponse.PSObject.Properties["result"] -and 
        $jsonResponse.result -eq 0 -and $jsonResponse.PSObject.Properties["webLoginExtensionIsAvailable"] -and 
        $jsonResponse.webLoginExtensionIsAvailable -eq $true) {
        Write-Host "Fetching the security key info..."
        $response = & accesskey access-automation get securitykey-info
        $response
        $jsonResponse = $response | ConvertFrom-Json

        if ($jsonResponse.PSObject.Properties["result"] -and 
            $jsonResponse.result -eq 0) {
            if ($jsonResponse.PSObject.Properties["lastUsedSecurityKeyInfo"] -and
                $jsonResponse.lastUsedSecurityKeyInfo.matchesTheLastPresented -eq $false) {
                Write-Host "New user card tapped..."
                Write-Host "Getting the extension's last action..."
                $response = & accesskey access-automation weblogin get last-action
                $response
                $jsonResponse = $response | ConvertFrom-Json
                if ($jsonResponse.PSObject.Properties["lastAction"] -and 
                $jsonResponse.lastAction -eq "login")
                {
                    Write-Host "Logging out the previous user..."
                    $response = & accesskey  access-automation weblogin execute automate --action "logout" --window "incognito" --url "https://myapps.microsoft.com?login_hint={UserId}" --method passkey -t 5000
                    $response
                }
                Write-Host "Logging in the new user..."
                $response = & accesskey  access-automation weblogin execute automate --action "login" --window "incognito" --url "https://myapps.microsoft.com?login_hint={UserId}" --method passkey -t 120000
                $response
            }
            else {
                Write-Host "Getting the last action..."
                $response = & accesskey access-automation weblogin get last-action
                $response
                $jsonResponse = $response | ConvertFrom-Json
                if ($jsonResponse.PSObject.Properties["lastAction"] -and 
                $jsonResponse.lastAction -eq "login")
                {
                    Write-Host "Logging out the user..."
                    $response = & accesskey  access-automation weblogin execute automate --action "logout" --window "incognito" --url "https://myapps.microsoft.com?login_hint={UserId}" --method passkey -t 1000
                    $response
                }
                else {
                    Write-Host "Logging in the user..."
                    $response = & accesskey  access-automation weblogin execute automate --action "login" --window "incognito" --url "https://myapps.microsoft.com?login_hint={UserId}" --method passkey -t 120000
                    $response
                }
            }
        }
    }
} catch {
    Write-Host "Error running the command: $_"
}
```

This script waits for Microsoft Edge for 60 seconds then brings it to the foreground and maximizes it. Then it waits for the IDmelon WebLogin extension for 10 seconds and onces it is available, it gets the security key information, then there are two conditions:

#### The presented security key is not the one previously used for login

In this case the script checks the WebLogin extension's last action. If the last action was `login`, it sends a `logout` command to the extension. Finally it sends the `login` command to the extension resulting the user being automatically logged in to the provided URL.

#### The presented security key is the one previously used for login

In this case, the script checks the WebLogin extension's last action. If the last action is `login`, it sends the `logout` command to the extension, otherwise, it sends the `login` command to the extension.

The result of the scripts execution can be found in this log file:
`"C:\ProgramData\IDmelon\Accesskey\power_shell_script_executer_logs.log"`
