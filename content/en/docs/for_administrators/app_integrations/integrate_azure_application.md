---
title: "Azure Application Integration"
description: "Integrating IDmelon with Azure Application"
lead: ""
date: 2023-12-15T23:05:00+03:30
lastmod: 2025-02-04T16:39:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "app_integrations"
weight: 15
toc: true
---

IDmelon makes it easy to integrate with your organization's Entra ID tenant using various features available in the IDmelon panel, such as importing users and devices. This document describes the integration process, details the necessary permissions, and explains how to grant admin consent for seamless operation.

## Overview

IDmelon integrates with your Entra ID tenant using app-only access, allowing it to function as a standalone application. This approach enables IDmelon to perform background tasks, such as importing users through workflows, without interruptions. Additionally, it eliminates the need for repeated permission approvals from privileged administrators.

For more details on access scenarios, refer to the official <a href="https://learn.microsoft.com/en-us/entra/identity-platform/permissions-consent-overview#access-scenarios" target="_blank">Microsoft documentation</a>.

## Required Permissions

Due to current Microsoft limitations, applications cannot request a subset of permissions for different actions. Consequently, IDmelon must request all permissions associated with its features. Learn more about this restriction in the <a href="https://learn.microsoft.com/en-us/entra/identity-platform/consent-types-developer#incremental-and-dynamic-user-consent" target="_blank">Microsoft consent documentation</a>.

The required permissions are as follows:

- `Device.Read.All`: Allows IDmelon to import all devices from Entra ID.
- `Group.Read.All`: Allows IDmelon to import all groups from Entra ID.
- `User.Read.All`: Allows IDmelon to read all users from Entra ID.
- `UserAuthenticationMethod.ReadWrite.All`: Allows IDmelon to add FIDO2 Security keys for users.

IDmelon always requests the least privileged permissions. However, if you are responsible for creating the permissions, you can substitute the first four permissions with the broader <a href="https://learn.microsoft.com/en-us/graph/permissions-reference#directoryreadall" target="_blank">Directory.Read.All</a> permission.

## Granting Consent

You can either grant permissions to the predefined IDmelon app registration with predefined permissions or define your own app registration manually and pass the required information to us.

### Using IDMelon’s App Registration

To approve these permissions, initiate any Entra ID-related feature within IDmelon. This action will redirect you to the admin consent flow. According to Microsoft, you need to sign in as at least a Privileged Role Administrator to consent to application permissions to Microsoft Graph.

For more details, refer to the official <a href="https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/grant-admin-consent?pivots=ms-graph#grant-admin-consent-for-application-permissions-using-microsoft-graph-api" target="_blank">Microsoft admin consent documentation</a>.

### Using Manual App Registration

If you need more control over the app registration and permissions, you can create your own app registration and integrate with `IDMelon`.

Navigate to `Azure Portal` and click on `Microsoft Entra ID`.
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_01.png)

From the App registrations blade, click on New registration.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_02.png)

Fill in a name for the app registration and choose Single tenant. Click Register.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_03.png)

In the Overview blade, copy the Application (client) ID and Directory (tenant) ID.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_04.png)

For security configuration, IDMelon supports both options that Microsoft provides in the `Certificates & Secrets` menu. In this part of the documentation, we explain both configuration options so you can choose one.

#### Client Secret

From the `Certificates & Secrets` blade, click the `Client secrets` tab, then click on `New client secret`. Add a description (optional), select the expiration, click Add, and copy the `secret value`.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_05.png)
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_06.png)
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_07.png)

#### Certificate

Another option for securing the Azure Application is using a **Certificate**. For this, you need to create `Private Key` and `Certificate` files, then upload the certificate to the Azure Portal and the private key to the IDMelon Administration Panel. To create these files in `RSA Format`, you have several options. We present two script examples using `Windows PowerShell` and `Linux Terminal`.

***Windows PowerShell Script***

In Windows, first create a PFX file using the following commands:

```powershell
# Create self-signed cert with exportable private key
$cert = New-SelfSignedCertificate -DnsName "localhost" `
    -CertStoreLocation "cert:\CurrentUser\My" `
    -KeyExportPolicy Exportable `
    -KeyLength 2048 `
    -KeySpec Signature `
    -NotAfter (Get-Date).AddYears(1)

# Export cert+private key as PFX (PKCS#12)
$pfxPath = "C:\Path\To\Output\File\mycert.pfx"
$securePwd = ConvertTo-SecureString -String "MyStrongPassword123!" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath $pfxPath -Password $securePwd
```
Now extract both Private Key and Certificate files using the following OpenSSL commands:
```powershell
# Convert private key to PEM (.key)
openssl pkcs12 -in C:\Path\To\Output\File\mycert.pfx -nocerts -out C:\Path\To\Output\File\private.key -nodes -passin pass:MyStrongPassword123!

# Convert certificate to PEM (.crt)
openssl pkcs12 -in C:\Path\To\Output\File\mycert.pfx -clcerts -nokeys -out C:\Path\To\Output\File\certificate.crt -passin pass:MyStrongPassword123!
```

***Linux Shell Script***

The following script creates `Private Key` and `Certificate` files using the `OpenSSL` library. It can install OpenSSL on a Debian-based environment if not already installed.
```bash
#!/bin/bash

#  This script is intended to run on a local machine with Ubuntu installed.

files=("private.key" "certificate.csr" "certificate.crt" "output.txt")

# check if OpenSSL is installed
if ! command -v openssl &> /dev/null
then
    echo "OpenSSL not found. Installing..."
    if sudo -v &> /dev/null; then
            echo "Sudo privileges found. Installing OpenSSL..."
            sudo apt update
            sudo apt install -y openssl
        else
            echo "No sudo privileges found. Cannot install OpenSSL."
        fi
else
    echo "OpenSSL is already installed."
fi

for file in "${files[@]}"; do
    if [ -f "$file" ]; then
        rm "$file"
    else
        echo "$file does not exist."
    fi
done

# Generate private key and self-signed certificate
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out private.key
openssl req -new -key private.key -out certificate.csr
openssl x509 -req -days 3650 -in certificate.csr -signkey private.key -sha256 -out certificate.crt # 10 years

echo "COMPLETED ;)"
```

**Upload Certificate**

After creation of `Private Key` and `Certificate` files, upload the `Certificate` in `Azure Portal`. From the `Certificates & Secrets` blade, click the `Certificate` tab, then click on `Upload certificate`. Select the certificate file, add a description (optional), then click Add and copy the `Thumbprint` (expand the column to see it completely).

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_08.png)
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_09.png)
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_10.png)

### Permissions Management

Navigate to the API permissions blade and click `Add a permission`. Select Microsoft `Graph` > `Application permissions`.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_11.png)

Select all the following permissions as required:

- `Device.Read.All`
- `Group.Read.All`
- `User.Read.All`
- `UserAuthenticationMethod.ReadWrite.All`

You can search for each of them and select it.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_12.png)

After adding all permissions, click `Grant admin consent for <domain>` and accept the confirmation.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_13.png)

Finally, you should see the following state in your Azure Application:

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_14.png)

### Configure Redirect URL

Go to the `Authentication` blade. Click `Add a platform` > `Web`. Enter `https://skm.idmelon.com/aad/redirect-azure` as the Redirect URI, then click Configure. Click `Add URI`, enter `https://panel.idmelon.com`, and click Save.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_15.png)

## Integration Azure Application with IDMelon

To integrate Azure Application with IDMelon, navigate to `App Integrations` > `Azure Application` and click on `New Application`.

![IDMelon Panel](/images/vendor/app_integration/azure_application/idmelon_01.png)

You have two options to integrate an Azure Application:  
1- Integrate using `Client Secret`, or  
2- Use `Certificate`, based on what you configured in `Azure Portal` previously.

### Using Client Secret

If you created a `Client Secret`, fill in the following fields based on the information in your `Azure Portal`:

- `Application (client) ID`: Copy this from the Azure Application Overview Page.
- `Directory (tenant) ID`: Also copy this from the Azure Application Overview Page.
- `Client Secret`: This is the `secret value` you created in the `Certificates and secrets` page earlier.

After filling in all fields, click `Test Configuration`. If you see a successful message, click `Create` to integrate your application.

![IDMelon Panel](/images/vendor/app_integration/azure_application/idmelon_02.png)

### Using Certificate

If you selected a `Certificate` option for your security policy, select the `Certificate` option in the `Azure Application` modal and fill in the following fields:

- `Application (client) ID`: Copy this from the Azure Application Overview Page.
- `Directory (tenant) ID`: Also copy this from the Azure Application Overview Page.
- `Private Key`: Upload the private key you created for the certificate.
- `Thumbprint`: This is available in the `Certificates and secrets` page under the `Certificate` tab. Select the thumbprint of the certificate relevant to the private key.

Finally, click `Test Configuration` to ensure your configuration can connect to Azure. If everything is fine, click `Create`.

![IDMelon Panel](/images/vendor/app_integration/azure_application/idmelon_02.png)
