---
title: "Deploy with PDQ Software"
description: "Easily deploy IDmelon software (e.g., reader drivers) to your organization using PDQ Deploy."
lead: ""
date: 2023-09-17T13:26:06+03:30
lastmod: 2023-09-17T13:26:06+03:30
draft: false
images: []
menu:
  docs:
    parent: "automatic_software_deployment"
weight: 92000
toc: true
---

Follow these steps to deploy products using PDQ on computers connected to AD:  

1. **Preparing the Setup Files**:  
    - Before starting the deployment, ensure the following setup files are placed in the specified directories:  
        - For the Pairing Tool:  
          `%PUBLIC%\Documents\Admin Arsenal\PDQ Deploy\Repository\PairingTool\Setup.exe`  
        - For the Access Key:  
          `%PUBLIC%\Documents\Admin Arsenal\PDQ Deploy\Repository\Accesskey\setup.exe`  

2. **Opening the Application**:  
    - Begin by launching the PDQ application on the admin system.  

3. **Importing the Package**:  
    - On the main screen, locate the "packages" folder in the left panel.  
    - Right-click on the "packages" folder.  
    - Select the "import" option from the context menu.  
    - Choose the relevant XML file for the application (either Pairing or Access Key).  

4. **Verifying the Package**:  
    - After importing, the name of the application (either Pairing or Access Key) should appear in the "packages" list. (Note: A screenshot was provided for reference.)  

5. **Deploying the Application**:  
    - Single-click on the application (either Pairing or Access Key) in the list.  
    - From the PDQ top menu, select the "Deploy Once" option.  
    - In the window that appears, on the left side, enter the names of the systems where the application will be installed.  
    - Finally, click on the "Deploy now" button.  

6. **Completion**:  
    - Wait for the application to be installed on all specified systems. Once done, your deployment is complete.  

### Updating Version  

While this is optional and does not affect the functionality of products, it is a good practice to update the version of the applications from the package details on each update. See the image below for more information:  

![version update](/images/vendor/how_to_deploy_products_using_pdq/versions.png)  
