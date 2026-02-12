---
title: "Deploy with Intune"
description: "Deploy IDmelon applications (e.g., reader drivers) using Microsoft Intune and .intunewin packages."
lead: ""
date: 2024-03-16T10:00:00+03:30
lastmod: 2024-03-16T10:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "automatic_software_deployment"
weight: 93000
toc: true
---

IDmelon exe software applications can be automatically deployed using Intune. Generally, exe file needs to be packaged in a format like .intunewin before deployment.  

## Step 1: Prepare the IntuneWin File  

Skip this step if you already have the intunewin package file.  

If you don’t have the file, follow the steps below to create the intunewin package.  

1. Download the [Microsoft Win32 Content Prep](https://github.com/microsoft/Microsoft-Win32-Content-Prep-Tool/archive/refs/heads/master.zip) tool and extract it into C:\IntuneContentPrepTool folder.  
2. Launch Powershell as an administrator and navigate to **C:\IntuneContentPrepTool**.  
3. Run **IntunewinAppUtil.exe** and provide Source Folder, Setup File, Output Folder, and Catalog Folder Information.  
    - **Source Folder**: Enter the location of the folder where .exe files / setup files are stored.  
    - **Setup File**: Enter the name of the .exe file.  
    - **Output folder**: Enter the location where .Intunewin file will be generated.  
    - **Catalog folder**: Type **N** and press Enter.  

![alt](/images/vendor/deploy_with_intune/01.jpg)
  
## Step 2: Create an App deployment on Intune  

Now, we have .Intunewin file created, Let’s check the deployment steps:  

1. Login on [Microsoft Intune admin center](https://intune.microsoft.com/)  
2. Click on **Apps** and then click on **All Apps**.  
3. Click on **+ Add** and Select **Windows app (Win32)** from the **app type**.  

## App Information  

Click on **“Select app package file**” and browse to **.Intunewin** file. Provide Information in below mandatory fields. The rest of the fields are optional, but can be useful for application documentation and troubleshooting issues.  

- Name: Accesskey Reader Driver
- Description: Accesskey Reader Driver
- Publisher: IDmelon
- App Version: 3.0.0.15

## Program  

Provide the Install command, uninstall command, Install behavior, Device restart behavior. Click on Next to proceed.  

- **Install command**: `accesskey_driver_3.0.0.15.exe /S`
- **Uninstall command**: `"C:\Program Files (x86)\IDmelon\Accesskey\uninstall.exe" /S`
- **Install behavior**: System

![alt](/images/vendor/deploy_with_intune/02.jpg)

## Detection rules  

Choose `Manually configure detections rules` for Rules format and then click on **Add** link to add file/path directory as below.  

- **Type**: File  
- **Path**: `C:\Program Files (x86)\IDmelon\Accesskey`  
- **File or folder**: `AccesskeyCli.exe`  
- **Detection method**: `File or folder exists`  
- **Associated with a 32-byte app**: No  

![alt](/images/vendor/deploy_with_intune/03.jpg)

## Dependencies  

Proceed with the **Next** as No dependency is needed.  

## Supersedence  

Proceed with the **Next**.

## Assignments  

Click on **Add group** to add an Azure AD group containing users or devices. You can also click on **Add all users** or **Add all devices**.  

## Review + create  

Review the deployment and click on Create to start the deployment process.  
