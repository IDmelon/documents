---
title: "Deploy with Windows Scripts"
description: "Deploy IDmelon applications (e.g., reader drivers) in your organization using Windows scripts."
lead: ""
date: 2024-02-29T01:00:00+03:30
lastmod: 2024-02-29T01:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "automatic_software_deployment"
weight: 91000
toc: true
---

This README provides a combined script for automatic deployment, including both installation and uninstallation procedures.  

## Installation  

To install the application silently, follow these steps:  

1. Open a command prompt with administrative privileges.  
2. Copy and paste the following command:  

```batch
@echo off
echo Installing Application...
start "" /wait "C:\Path\to\Your\Application\setup.exe" /S
echo Installation complete.
```

Replace `"C:\Path\to\Your\Application\setup.exe"` with the actual path to your installation executable and adjust any parameters (/S for silent installation) as necessary.  

## Uninstallation  

To uninstall the application silently, follow these steps:  

1. Open a command prompt with administrative privileges.  
2. Copy and paste the following command:  

```batch
@echo off
echo Uninstalling Application...
start "" /wait "C:\Program Files (x86)\IDmelon\Accesskey\uninstall.exe" /S
timeout /t 30 /nobreak >nul
echo Uninstallation complete.
```

Replace `"C:\Program Files (x86)\IDmelon\Accesskey\uninstall.exe"` with the actual path to the uninstaller executable and adjust any parameters (/S for silent uninstallation) as necessary. You can also adjust the timeout duration (/t 30 in seconds) as needed.  
