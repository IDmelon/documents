---
title: "Deploying IDmelon Kiosk Using Intune"
description: "Deploying IDmelon Kiosk"
lead: ""
date: 2022-02-16T18:14:02-08:00
lastmod: 2022-02-16T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "idmelon_kiosk"
weight: 20
toc: true
---

This guide provides step-by-step instructions for deploying IDmelon Kiosk for Kiosk Mode Policy using Microsoft Intune.

## Step 1: Add IDmelon Kiosk App to Intune

1. Access the Endpoint Manager [Microsoft Intune](https://intune.microsoft.com).
2. Navigate to **Apps > All apps**.
3. Add a New Microsoft Store App
    - Click on **+ Create** at the top of the screen.
    - From the dropdown, select **Microsoft Store app (new)**.
    - Search for **IDmelon Kiosk** in the Microsoft Store and select it.
    - In the **App Information** pane, click **Search the Microsoft Store app (new)**.
    - A search window will appear.
    - In the search field, type **IDmelon Kiosk**.
    - Press **Enter**.
    - Locate IDmelon Kiosk from the search results.
    - Click on the app name to select it.
    ![IDmelon Kiosk from MS Store](/images/vendor/idmelon_kiosk/kiosk-app-choose-idemlon-store.png)
4. Configure App Information
    - Change the **Install behavior** to **System**
    - Click **Next** to continue.
    ![IDmelon Kiosk Installation Behavior](/images/vendor/idmelon_kiosk/intune-kiosk-app-system.png)
5. Assignments:
    - Decide who gets the app.
        - Under **Required**, click **Add Group** and select the device or user groups you want to deploy the app to.
    - Click **Next** when done.
    ![IDmelon Kiosk Assignment](/images/vendor/idmelon_kiosk/Intune-device-group-assignment.png)
6. Review + Create:
    - Review all the configurations you've made.
    - Click **Create** to add the app and its dependencies to Intune.

## Step 2: Create a Kiosk Configuration Policy

1. Navigate to Configuration Profiles:
    - Go to **Devices > Configurations**.
2. Create a Kiosk Policy:
    - Click **+ Create > New Policy**.
    - Platform: Choose **Windows 10 and later**.
    - Profile type: Select **Templates > Kiosk**.
    ![Create Kiosk Policy](/images/vendor/idmelon_kiosk/Intune-adding-kiosk-policy.png)
3. Configure Basics:
    - Name: Enter **IDmelon Kiosk Configuration**.
    - Description: Provide details to identify the profile (optional).
    - Click **Next** to proceed.
4. Configuration Settings:
    - Kiosk mode: Select **Multi-app kiosk**.
    - Target devices running Windows 10/11 in S mode: Select **No**.
    - User logon type: Choose **Autologon**.
    - Browsers and Applications: Select **Add by AUMID**.
    - App Name: Enter **IDmelon Kiosk**.
    - AUMID: Enter **Hellokey.45853B8ADE74A_kxcedb3gts26c!App**.
    - Click **OK** to add the app to the kiosk profile.
    - Click **Next** to continue.
    ![Create Kiosk Policy](/images/vendor/idmelon_kiosk/Intune-adding-kiosk-policy-config-idmelon-kiosk-app.png)
5. Assignments:
    - Under Included groups, click **Add groups**.
    - Select the device groups that should receive the kiosk configuration.
    ![Create Kiosk Policy](/images/vendor/idmelon_kiosk/Intune-adding-kiosk-policy-config-assignment.png)
6. Review + Create:
    - Click **Create** to deploy the configuration profile.

## Step 3: Configure the Kiosk Settings

Editing the **configs.xml** file enables you to tailor the IDmelon Kiosk app to your preferences.

1. Navigate to the App's Local State Folder:
    - In the File Explorer address bar, type or paste the following path and press Enter:

    ```shell
    %LOCALAPPDATA%\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState
    ```

    For kiosk users, the path is:

    ```shell
    C:\Users\kioskUser0\AppData\Local\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState
    ```

    - This path directs you to the **LocalState** folder where the **configs.xml** file is stored.

### Configuration Keys

```xml
<KioskURLs>
    <URL auth_required="true">https://myapps.microsoft.com</URL>
</KioskURLs>
<SelfService>false</SelfService>
<MultiTabMode>true</MultiTabMode>
<ExtensionEnabled>true</ExtensionEnabled>
<EndSessionConfirmation>false</EndSessionConfirmation>
<RestartAppOnEndSession>true</RestartAppOnEndSession>
<ShowURLBar>false</ShowURLBar>
<ShowEndSessionButton>true</ShowEndSessionButton>
<ServerAddress env="prod" />
<ShowFeedbackButton>true</ShowFeedbackButton>
<ExtensionAutoUpdate>true</ExtensionAutoUpdate>
<ExtensionUpdateChannel>stable</ExtensionUpdateChannel>
<AppAutoUpdate silent="false">true</AppAutoUpdate>
<FeedbackEmails from="" to="support@idmelon.com" cc="" />
<KioskIdleTimeoutSeconds>0</KioskIdleTimeoutSeconds>
<UserInteractionMonitoringAppPath>C:\Program Files (x86)\IDmelon\Accesskey</UserInteractionMonitoringAppPath>
<KeepDisplayAwake>false</KeepDisplayAwake>
<Policies>
    <URLAllowList />
    <URLBlockList />
    <URLBarEditingEnabled>false</URLBarEditingEnabled>
</Policies>
<ForceNewTab>true</ForceNewTab>
<CustomPagesURL auto_update="true"></CustomPagesURL>
```

- **KioskURLs:** The addresses that must be opened when the kiosk app starts.
- **SelfService:** Set it true if the **KioskURLs** are anything other than myapps.microsoft.com.
- **MultiTabMode:** Enables (true) or disables (false) multi-tab browsing. Setting this to false activates full-screen mode.
- **ExtensionEnabled:** Toggles browser extensions on (true) or off (false).
    Note: The Browser extension is required for the automation process. (When a card taps on the reader, the login automation will start).
- **EndSessionConfirmation:** Prompts users for confirmation when ending a session if set to true.
- **RestartAppOnEndSession:** Restarts the Kiosk app on each end session to reload the app configs.
- **ShowURLBar:** Displays (true) or hides (false) the browser's URL bar.
- **ShowEndSessionButton:** Displays (true) or hides (false) the end session button.
- **ServerAddress:** Target server address (for dedicated environments).
- **ShowFeedbackButton:** Displays (true) or hides (false) the feedback (report issue) button next to the end session button.
- **ExtensionAutoUpdate:** Enables (true) or disables (false) Weblogin extension auto updates.
- **ExtensionUpdateChannel:** The update channel of the Weblogin extension (e.g., stable or latest).
- **AppAutoUpdate:** Enables (true) or disables (false) app updates. (silent=true -> Updates without displaying a notification UI to the user).
- **FeedbackEmails:** Customizing support emails for issue reporting.
- **KioskIdleTimeoutSeconds:** Set the session idle timeout (in seconds). When the timeout occurs, all session information will be cleared, and the kiosk will be ready for the next user to use. set it to 0 to disable it.
- **UserInteractionMonitoringAppPath:** The path of the User Interaction Monitoring app. If the value of the `KioskIdleTimeoutSeconds` is set to 0, there is no need to set this address.
- **KeepDisplayAwake:** Controls whether the system is allowed to follow normal power‑saving rules (default = false) or whether it should keep the display awake at all times.
- **Policies:** Configurable rules that define the app’s behavior and user experience, such as allowed/blocked URLs, editing permissions, and security limits. (see [Policies](#policies) for more details)
- **CustomPagesURL:** The address of your custom **Report Issue** pages package, so the kiosk can replace the built-in report dialog with your own HTML pages. Leave it empty to keep the built-in dialog. (see [Report Issue Custom Pages](#report-issue-custom-pages) for more details)

#### Kiosk URLs

Add your URLs inside `<KioskURLs>` as `<URL>` elements.
If the URL requires authentication, set `auth_required="true"`.

If multiple URLs are authenticated by the same platform, set `auth_required="true"` on only one of them. For example:

```xml
<KioskURLs>
    <URL auth_required="true">https://myapps.microsoft.com</URL>
    <URL auth_required="false">https://excel.cloud.microsoft</URL>
    <URL auth_required="false">https://idmelon.com</URL>
</KioskURLs>
```

#### Configuring Server Address

The IDmelon Kiosk app allows you to specify the target server address using the ServerAddress tag in the configuration file. By default, it connects to the IDmelon server. If you need to connect to a dedicated environment, follow these steps:

- Locate the ServerAddress tag in the config file. By default, it looks like this:

```xml
<ServerAddress env="prod" />
```

- To connect the app to a custom server (dedicated environments), modify the attributes as follows:

```xml
<ServerAddress env="onpremise" base-api="https://sub.domain.com/api/url" />
```

#### Weblogin Extension Auto Update

Automatic extension update is enabled by default. To **disable** it, set the following config to **false**:

```shell
<ExtensionAutoUpdate>false</ExtensionAutoUpdate>
```

**Note:** Disabling automatic updates may prevent the application of important bug fixes and new feature enhancements.

#### Weblogin Extension Update Channel

The default extension update channel is set to **Stable**. To receive newer versions, such as **Latest**, update the following config in the app configs:

```shell
<ExtensionUpdateChannel>stable</ExtensionUpdateChannel>
```

The available channels are **Stable** and **Latest**.

#### Policies

Policies includes the following configurations:

- `URLAllowList` → List of allowed URLs
- `URLBlockList` → List of blocked URLs
- `URLBarEditingEnabled` → Controls whether users can edit the address bar

Examples for the `URLAllowList` and `URLBlockList`:

> Add each URL between the `<URL></URL>` tags.<br>
Use `*` as a wildcard to match any sequence of characters, and `?` to match a single character.

- All URLs are allowed:

    ```xml
    <URLAllowList />
    <URLBlockList />
    ```

- All URLs are blocked:

    ```xml
    <URLAllowList />
    <URLBlockList>
        <URL>*</URL>
    </URLBlockList>
    ```

- Block all domains except idmelon.com:

    ```xml
    <URLAllowList>
        <URL>https://*idmelon.com/*</URL>
    </URLAllowList>
    <URLBlockList>
        <URL>*</URL>
    </URLBlockList>
    ```

- All domains are allowed except idmelon.com:

    ```xml
    <URLAllowList />
    <URLBlockList>
        <URL>https://*idmelon.com/*</URL>
    </URLBlockList>
    ```

- Only Microsoft and IDmelon domains are allowed:

    ```xml
    <URLAllowList>
        <URL>https://*idmelon.com/*</URL>
        <URL>https://*microsoft.com/*</URL>
        <URL>https://*microsoftonline.com/*</URL>
    </URLAllowList>
    <URLBlockList>
        <URL>*</URL>
    </URLBlockList>
    ```

#### Weblogin Extension Configuration

If the **Weblogin Extension** is enabled in the kiosk configuration, you can also save the extension configuration as a **JSON** file next to the **configs.xml**.

```json
{
    "handlePasskeyRequests": {
        "value": true
    },
    "allowAddingNewPasswords": {
        "value": true
    },
    "allowPasswordUpdatePrompts": {
        "value": true
    },
    "useEmbeddedNumpad": {
        "value": false
    },
    "keystroking": {
        "value": true
    }
}
```

Save this JSON string as **extension_configs.json** in the following path:

```shell
    C:\Users\kioskUser0\AppData\Local\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState
```

## Step 4: Deploy Configuration

There are two deployment methods available:

- Automatically using script – copies the configs.xml file directly into each kiosk user profile via an MDM provider (e.g., Microsoft Intune).
- Automatically using IDmelon Accesskey – applies the configuration in Base64 format through the Accesskey CLI.
    > This method requires the IDmelon Accesskey to be installed on PCs.

### Automatically using script

Convert the **configs.xml** file into a base64 string using the following command:

```bash
# configs.xml: the config file path that you want to deploy on target PCs
[Convert]::ToBase64String([IO.File]::ReadAllBytes('configs.xml')) | Set-Clipboard
```

Replace the generated base64 string in the following script and save the script as a  .ps1 (PowerShell script) file:

```bash
#region Configuration
$configBase64   = 'PASTE_BASE64_HERE'
$configFileName = 'configs.xml'
$appPackage     = 'Hellokey.45853B8ADE74A_kxcedb3gts26c'
$markerRelPath  = "AppData\Local\IDmelon\KioskConfig\$configFileName.deployed"
#endregion

$configBytes = [Convert]::FromBase64String($configBase64)

# Version stamp derived from the payload itself.
$sha = [System.Security.Cryptography.SHA256]::Create()
try {
    $configVersion = ([BitConverter]::ToString($sha.ComputeHash($configBytes)) -replace '-').Substring(0, 16)
} finally {
    $sha.Dispose()
}
Write-Output "Payload version: $configVersion"

$profilesPath = "C:\Users"
$kioskUserDirectories = Get-ChildItem -Path $profilesPath -Directory -Filter "kioskUser*"

if ($kioskUserDirectories.Count -eq 0) {
    Write-Output "No kioskUser* profiles found under $profilesPath"
    exit 1
}

$copied = 0; $skipped = 0; $failures = 0

foreach ($userDir in $kioskUserDirectories) {
    $name       = $userDir.Name
    $markerFile = Join-Path -Path $userDir.FullName -ChildPath $markerRelPath
    $targetDir  = Join-Path -Path $userDir.FullName -ChildPath "AppData\Local\Packages\$appPackage\LocalState"
    $targetFile = Join-Path -Path $targetDir -ChildPath $configFileName

    # Skip if the recorded version matches AND the config is actually still there
    if ((Test-Path -LiteralPath $markerFile) -and (Test-Path -LiteralPath $targetFile)) {
        $recorded = (Get-Content -LiteralPath $markerFile -TotalCount 1 -ErrorAction SilentlyContinue)
        if ($recorded -and $recorded.Trim() -eq $configVersion) {
            Write-Output "[$name] Already at version $configVersion - skipping."
            $skipped++
            continue
        }
        Write-Output "[$name] Recorded version '$($recorded)' differs from '$configVersion' - updating."
    }

    try {
        New-Item -Path $targetDir -ItemType Directory -Force | Out-Null
        [System.IO.File]::WriteAllBytes($targetFile, $configBytes)

        # Marker written only after a successful copy
        New-Item -Path (Split-Path -Path $markerFile -Parent) -ItemType Directory -Force | Out-Null
        Set-Content -LiteralPath $markerFile -Encoding UTF8 -Value @(
            $configVersion
            "DeployedUtc = $((Get-Date).ToUniversalTime().ToString('o'))"
            "Target      = $targetFile"
        )

        Write-Output "[$name] Deployed version $configVersion to: $targetFile"
        $copied++
    } catch {
        Write-Output "[$name] Failed: $_"
        $failures++
    }
}

Write-Output "Summary: $copied deployed, $skipped skipped, $failures failed."
if ($failures -gt 0) { exit 1 } else { exit 0 }
```

#### Create a PowerShell script policy in Intune

- Navigate: **Devices > Windows > Scripts and remediations > Platform Scripts**.
- Select **Add**.
- Basics: Name the script (e.g., “Deploy Kiosk configs.xml”).
- Select **Next**.
- Upload script: Select your **.ps1** file.
- Run this script using the logged-on credentials: **No** (run as System).
- Enforce script signature check: **No**.
- Run script in 64-bit PowerShell: **Yes** (recommended on Windows 11).
- Assignments: Assign to desired groups.
- Review + add: Confirm and create the script deployment.

### Automatically using IDmelon Accesskey

Starting with Accesskey version 3.9.0, you can set the kiosk configuration in base64 format:

To convert the **configs.xml** to a base64 string, enter the following command in a PowerShell:

```shell
[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes((Get-Content -Raw -Path "configs.xml"))) > "output.txt"
```

Replace the content of the file **output.txt** in the following command:

```shell
accesskeycli kiosk -s -c [Base64-String]
```

You can also do the above steps for the **Weblogin Extension** configs:

```shell
[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes((Get-Content -Raw -Path "configs.json"))) > "output.txt"
```

```shell
accesskeycli kiosk -s -c [Kiosk-Configs-Base64-String] -e [Extension-Configs-Base64-String]
```

## Step 4: Configure the Automation Workflow (Optional)

Run the below command in either Command Prompt or PowerShell to configure login-logout flow for Microsoft.

With Command Prompt:

```cmd
accesskeycli workflow-automation -s -t weblogin-extension -a login-logout -u "https://myapps.microsoft.com?login_hint={UserId}" -m passkey
```

With PowerShell:

```powershell
accesskeycli workflow-automation -s -t weblogin-extension -a login-logout -u "https://myapps.microsoft.com?login_hint={UserId}" -m passkey
```

## Step 5: Set the Self-Service URL (Optional)

If you have set up **Self-Service** settings in the **IDmelon Admin Panel**, enter the following command in a PowerShell:

```shell
accesskeycli self-service-link -s [Self-Service URL]
```

By executing this command, the system will automatically handle specific user scenarios.<br>
If the user’s badge is **not enrolled**, they will be redirected to the designated enrollment page. Similarly, if the user exceeds the allowed number of incorrect PIN attempts, they will be automatically directed to the specified address for a **PIN reset**.

## Report Issue Custom Pages

By default, when a user taps the feedback (**?**) button or something goes wrong during sign-in, the kiosk shows its own built-in **Report Issue** dialog. You can replace that dialog with your own HTML pages — your wording, your branding, your fields — and the kiosk sends the submitted form to your support address exactly as it does for the built-in one.

Each page is a plain HTML file. The kiosk serves the pages from a local virtual host, injects a small JavaScript bridge into them, and waits for the page to post the filled-in form back.

If the page for an event is missing, the kiosk falls back to the built-in dialog, so a partial package is safe to deploy.

### Supported pages

| Page | Shown when |
| ---- | ---------- |
| `automation_failed.html` | The sign-in automation could not be completed |
| `badge_locked.html` | The user's card is locked |
| `badge_enrollment.html` | The card is not enrolled yet |
| `report_issue.html` | The user pressed the feedback (**?**) button |

These names are only the defaults — see [The manifest](#the-manifest) to use your own.

> The Weblogin extension is told to report its errors to the kiosk automatically, but only when a page exists for at least one of the three failure events. A package carrying only `report_issue.html` leaves the extension to handle its own errors. You no longer need to set `sendErrorsToKiosk` in **extension_configs.json**.

### Page structure

The kiosk injects `window.idmelonKiosk` into every page before it loads:

```js
window.idmelonKiosk = {
  eventType,                    // "OnAutomationFailed" | "OnBadgeLocked" | "OnEnrollmentNeeded" | "General"
  details,                      // optional context string about what happened, or null
  variables,                    // predefined values, e.g. { badgeId, userId } - always an object
  onResult: null,               // assign a callback to receive the submission result
  submitReport(form, options),  // send the report
  close()                       // dismiss the page
};
```

A complete minimal page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>Your card is locked</title>
</head>
<body>
    <form id="reportForm">
        <h1>Your card is locked</h1>
        <p>Complete the form below and click <b>Send report</b> to notify the HelpDesk.</p>

        <!-- data-idmelon-var fills this field in automatically, before your own scripts run -->
        <label for="userId">User ID</label>
        <input type="text" id="userId" data-idmelon-var="userId" disabled />

        <label for="note">Note</label>
        <textarea id="note"></textarea>

        <button type="button" id="cancelButton">Not now</button>
        <button type="submit">Send report</button>
    </form>

    <script>
        document.getElementById("cancelButton").addEventListener("click", function () {
            window.idmelonKiosk.close();
        });

        // Called by the kiosk once the report has been sent
        window.idmelonKiosk.onResult = function (result) {
            alert(result.success ? "Report sent." : result.message);
            if (result.success) { window.idmelonKiosk.close(); }
        };

        document.getElementById("reportForm").addEventListener("submit", function (event) {
            event.preventDefault();

            window.idmelonKiosk.submitReport({
                title: "IDmelon Kiosk - PIN Locked",
                description: "The user's card is locked.",
                form: {
                    userId: document.getElementById("userId").value,
                    note: document.getElementById("note").value
                }
            }, { includeScreenshot: false });
        });
    </script>
</body>
</html>
```

#### Using the variables

The kiosk passes predefined values to the page in `window.idmelonKiosk.variables`. Two are available today:

- `badgeId` — the card the event was raised for
- `userId` — the signed-in user

The simplest way to use them is to tag an element with `data-idmelon-var`. The kiosk fills it in before your own scripts run:

```html
<input type="text" id="userId" data-idmelon-var="userId" disabled />
<span data-idmelon-var="badgeId"></span>
```

Inputs, textareas and selects get their `value` set; anything else gets its text. An element whose variable was **not** provided is left alone, so the default you ship keeps working. You can also read the object directly:

```js
var badge = window.idmelonKiosk.variables.badgeId || "";
```

There is no need to include the variables in your form — the kiosk appends them to every report under a **Badge Info** heading.

#### The submitted form

`submitReport(form, options)` takes any object you like; the whole thing is written into the report, so nothing is dropped. Field names become readable headings — `fullName` is shown as **Full Name**, `badge_number` as **Badge Number**.

Three keys are treated specially at the top level:

| Key | What it does |
| --- | ------------ |
| `title` or `subject` | Becomes the subject of the report email. Not repeated in the body. |
| `description` | Written as a plain sentence, with no heading of its own. |
| `form` | Shown under a **User's Form** heading — the fields the user filled in. |

`options` accepts `includeScreenshot` only, and it defaults to `false`. The app logs and configuration are attached to every report regardless.

The result of the submission is posted back to `window.idmelonKiosk.onResult` as `{ success, message }`. The kiosk never closes the page on its own — call `window.idmelonKiosk.close()` when you are done. A native close button is always available in the top-right corner as a fallback.

### The manifest

A package is a **manifest.json** file plus one folder per category:

```shell
manifest.json
ReportIssue\
    automation_failed.html
    badge_locked.html
    badge_enrollment.html
    report_issue.html
    assets\
        report.css
        logo.png
```

The manifest names the page that serves each event, so your files can be called anything:

```json
{
    "manifest_version": "1.0.0",
    "files_version": "1.0.0",
    "Pages": {
        "ReportIssue": {
            "automation_failed": "automation_failed.html",
            "badge_locked": "badge_locked.html",
            "badge_enrollment": "badge_enrollment.html",
            "report_issue": "report_issue.html"
        }
    }
}
```

- **manifest_version:** The manifest layout. Only the major part is checked; the current version is `1`.
- **files_version:** Your own version for the content. The kiosk logs it and otherwise ignores it. Bump it when you publish an update.
- **Pages:** Category → page key → file name, relative to the category folder.

Notes:

- Page keys are `automation_failed`, `badge_locked`, `badge_enrollment` and `report_issue`, matched case-insensitively.
- A page you leave out falls back to its default file name, so list only what you renamed.
- A file name may sit in a subfolder (`en-us/locked.html`) but must stay inside the category folder and end in `.html`.
- Only the `ReportIssue` folder is served to the browser, so the manifest itself is never reachable from a page.
- Relative assets (`assets/report.css`, `logo.png`, …) placed next to the pages work as normal.

The package is installed in the app's **LocalState** folder, so you can also copy it in by hand on a single PC:

```shell
    C:\Users\kioskUser0\AppData\Local\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\CustomPages
```

### Publishing the package

For more than a handful of PCs, publish the package as a **zip** and point the kiosks at it. Zip the **contents** of the package folder, so that `manifest.json` sits at the root of the archive:

```shell
manifest.json
ReportIssue/report_issue.html
ReportIssue/assets/report.css
```

The zip can be served from anywhere the kiosk PC can reach:

| Source | Example |
| ------ | ------- |
| HTTPS | `https://files.contoso.com/kiosk/report-pages.zip` |
| HTTP | `http://192.168.1.10/kiosk/report-pages.zip` |
| Shared folder (UNC) | `\\192.168.1.10\Deploy\report-pages.zip` |
| Local folder | `C:\Users\kioskUser0\AppData\Local\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\report-pages.zip` |

> A shared folder is usually the least work on a domain-joined fleet: no web server to stand up, and the existing share permissions control who can read it. Prefer HTTPS or a share over plain HTTP, which is unauthenticated and open to tampering.

Each kiosk fetches the package once per launch, in the background — the app starts whether or not the source is reachable. On a web server the check is a conditional `GET`, so an unchanged package costs a `304`; for a shared or local folder the size and modified time are compared.

**A package is only installed if it is complete and valid.** It is downloaded, extracted to a staging folder and checked before anything replaces the live pages, so a truncated download, the wrong zip, or a manifest pointing at missing files all leave the running pages untouched. A package is refused when it:

- has no usable `manifest.json` at its root,
- maps no page that actually exists under `ReportIssue`,
- contains a file type that is not markup or an asset (`.exe`, `.ps1`, `.dll`, …),
- contains a path that would escape the folder,
- is over 20 MB zipped, 100 MB extracted, or 1000 files.

Every outcome is written to the app log, so a rejected package is visible on the machine.

To roll out a change: edit the files, bump `files_version`, re-zip, and replace it at the same address. Kiosks pick it up the next time they launch.

### Configuring the package source

Add the address to the kiosk configuration (Refer to [Configurations](#step-3-configure-the-kiosk-settings)):

```xml
<CustomPagesURL auto_update="true">https://files.contoso.com/kiosk/report-pages.zip</CustomPagesURL>
```

- **Element value:** Where the zip lives — an `https://` or `http://` URL, a shared folder (`\\192.168.1.10\Deploy\report-pages.zip`), or a local path. Leave it empty to use whatever is already installed on the PC, or nothing at all.
- **auto_update:** Set it to `false` to stop the kiosk fetching the package. Defaults to `true`.
