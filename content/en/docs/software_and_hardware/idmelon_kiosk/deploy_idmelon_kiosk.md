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
<KioskURL>https://myapps.microsoft.com</KioskURL>
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
```

- **KioskURL:** The default webpage the kiosk app loads upon startup.
- **SelfService:** Set it true if the **KioskURL** is a custom url (e.g., self-service url).
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

Below script can be used to copy the config.xml files to any Kiosk user profile. Save it as a ps1 (PowerShell script) file.

```bash
# Define the source file to copy
$sourceFile = "C:\KioskProfile\configs.xml"  # Update this to the path of the file you want to copy

# Define the base path for user profiles & get all user directories that start with "kioskUser0"
$profilesPath = "C:\Users"
$kioskUserDirectories = Get-ChildItem -Path $profilesPath -Directory -Filter "kioskUser0*"

# Loop through each kioskUser0 directory
foreach ($userDir in $kioskUserDirectories) {
    # Define the target directory for this user
    $targetDirectory = Join-Path -Path $userDir.FullName -ChildPath "AppData\Local\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState"

    # Create the target directory if it doesn't exist
    if (-not (Test-Path -Path $targetDirectory)) {
        New-Item -Path $targetDirectory -ItemType Directory | Out-Null
        Write-Host "Created directory for user $($userDir.Name) at: $targetDirectory"
    }

    # Define the target file path
    $targetFile = Join-Path -Path $targetDirectory -ChildPath (Split-Path -Path $sourceFile -Leaf)

    # Copy the file
    try {
        Copy-Item -Path $sourceFile -Destination $targetFile -Force
        Write-Host "File copied successfully for user $($userDir.Name) to: $targetFile"
    } catch {
        Write-Host "Failed to copy file for user $($userDir.Name): $_"
    }
}
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
accesskeycli workflow-automation -s -t weblogin-extension -a login-logout -u https://myapps.microsoft.com?login_hint=${UserId} -m passkey
```

With PowerShell:

```powershell
accesskeycli workflow-automation -s -t weblogin-extension -a login-logout -u https://myapps.microsoft.com?login_hint='${UserId}' -m passkey
```

*Note: Since the login_hint attribute is passed as a parameter, PowerShell requires the ${} syntax to be enclosed in single quotes, like '${UserId}'.*

## Step 5: Set the Self-Service URL (Optional)

If you have set up **Self-Service** settings in the **IDmelon Admin Panel**, enter the following command in a PowerShell:

```shell
accesskeycli self-service-link -s [Self-Service URL]
```

By executing this command, the system will automatically handle specific user scenarios.<br>
If the user’s badge is **not enrolled**, they will be redirected to the designated enrollment page. Similarly, if the user exceeds the allowed number of incorrect PIN attempts, they will be automatically directed to the specified address for a **PIN reset**.
