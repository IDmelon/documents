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

## Granting Consent

You can either grant permissions to the predefined IDmelon app registration with predefined permissions or define your own app registration manually and pass the required information to us.

### Using Default App Registration

To approve these permissions, initiate any Entra ID-related feature within IDmelon. This action will redirect you to the admin consent flow. According to Microsoft, you need to sign in as at least a Privileged Role Administrator to consent to application permissions to Microsoft Graph.

For more details, refer to the official <a href="https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/grant-admin-consent?pivots=ms-graph#grant-admin-consent-for-application-permissions-using-microsoft-graph-api" target="_blank">Microsoft admin consent documentation</a>.

### Using Manual App Registration

If your organization restricts permission approvals from external applications or requires more control over app registration and permissions, you can create your own app registration and integrate it with IDmelon. To do so, follow the steps below:

Navigate to `Azure Portal` and click on `Microsoft Entra ID`.
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_01.png)

From the App registrations blade, click on `New registration`.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_02.png)

Fill in a `Name` for the app registration and choose `Accounts in this organizational directory only`. Click `Register`.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_03.png)

In the Overview blade, copy the `Application (client) ID` and `Directory (tenant) ID`.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_04.png)

Navigate to the API permissions blade and click `Add a permission`. Select `Microsoft Graph` > `Application permissions`.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_11.png)

Select following permissions:

- `Device.Read.All`
- `Group.Read.All`
- `User.Read.All`
- `UserAuthenticationMethod.ReadWrite.All`

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_12.png)

> You can also substitute all these permissions with the broader <a href="https://learn.microsoft.com/en-us/graph/permissions-reference#directoryreadall" target="_blank">Directory.Read.All</a> permission.
> If you want to restrict the `UserAuthenticationMethod.ReadWrite.All` permission to a specific subset of users in your organization, you can skip adding this permission at this stage and follow the instructions below to configure it using Administrative Units.

After adding all permissions, click `Grant admin consent for <domain>` and accept the confirmation.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_13.png)

Finally, you should see the following state in your Azure Application:

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_14.png)

If your organization wants to limit the `UserAuthenticationMethod.ReadWrite.All` permission to specific user groups, you can use `Administrative Units` instead of granting this permission globally. This allows you to scope the permission more precisely, giving IDmelon access only to manage authentication methods for users within the defined units. To configure scoped permissions:

1. Navigate to **Azure Portal** > **Entra ID**.
2. Go to **Roles and Administrators**.
3. Search for and select the **Authentication Administrator** role.
4. Click **Add Assignments**.
5. In the assignment configuration:
   - Set **Scope type** to `Administrative unit`.
   - Select your administrative unit for the **Selected Scope**.
   - Under **Selected member(s)**, choose the IDmelon application you created in Enterprise Applications.
     ![Entra ID - Authentication Administrator Assignment](/images/vendor/app_integration/azure_application/azure_app_scoped_assign_1.png)
6. Click **Next**.
7. Provide a detailed justification. For example: `Granting the permanent Authentication Administrator role to IDmelon to manage Passkey rollout for targeted users and groups defined within the Administrative Units.`
8. Complete the assignment.

This configuration allows IDmelon to manage MFA authentication methods only for users within the specified administrative unit, rather than all users in your organization.

For detailed instructions on creating and managing Administrative Units, refer to the [Microsoft documentation](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/admin-units-manage?tabs=admin-center#create-an-administrative-unit).

For security configuration, IDmelon supports both certificate-based authentication and client secret–based authentication.

#### Client Secret

From the Certificates & Secrets blade, click the `Client secrets` tab, then click on `New client secret`. Add a description (optional), select the expiration, click Add, and copy the `secret value`.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_05.png)
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_06.png)
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_07.png)

#### Certificate

To integrate with IDmelon using certificate-based authentication, you need to generate a private key and a certificate, upload the certificate to the Azure portal, and upload the private key to the IDmelon Administration Panel.

Run the following PowerShell commands:

```powershell
# Create a self-signed certificate with an exportable private key
$cert = New-SelfSignedCertificate -DnsName "localhost" `
    -CertStoreLocation "cert:\CurrentUser\My" `
    -KeyExportPolicy Exportable `
    -KeyLength 2048 `
    -KeySpec Signature `
    -NotAfter (Get-Date).AddYears(2)
```

> Adjust the certificate expiration period by changing AddYears(2) as needed.

```powershell
# Export the certificate and private key as a PFX (PKCS#12) file
$pfxPath = "C:\Path\To\Output\File\mycert.pfx"
$securePwd = ConvertTo-SecureString -String "MyStrongPassword123!" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath $pfxPath -Password $securePwd
```

> Change `$pfxPath` to the file path where you want to save the PFX file.
> Set a strong password in place of `MyStrongPassword123!` to protect the PFX file.

Use OpenSSL to extract the private key and certificate files in PEM format:

```powershell
# Extract the private key (PEM format)
openssl pkcs12 -in C:\Path\To\Output\File\mycert.pfx -nocerts -out C:\Path\To\Output\File\private.key -nodes -passin pass:MyStrongPassword123!

# Extract the certificate (PEM format)
openssl pkcs12 -in C:\Path\To\Output\File\mycert.pfx -clcerts -nokeys -out C:\Path\To\Output\File\certificate.crt -passin pass:MyStrongPassword123!
```

> Update the input PFX path (-in) to where your PFX file is saved.
> Specify the desired output paths (-out) for the private key and certificate files.
> Use the password you set when exporting the PFX in the -passin argument.

After creating the private key and certificate files, upload the `certificate` to the Azure portal. In the Certificates & Secrets blade, select the `Certificates` tab, then click `Upload certificate`. Choose your certificate file, optionally add a description, click Add, and copy the `Thumbprint` (you may need to expand the column to see it fully).

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_08.png)
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_09.png)
![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_10.png)

### Configure Redirect URL

Go to the `Authentication` blade. Click `Add a platform` > `Web`. Enter `https://skm.idmelon.com/aad/redirect-azure` as the Redirect URI, then click Configure. Click `Add URI`, enter `https://panel.idmelon.com` and `https://panel.idmelon.com/msal/result`, and click Save.

![Azure Portal](/images/vendor/app_integration/azure_application/azure_app_15.png)

## Adding the Application in IDmelon Admin Panel

In IDmelon Admin Panel, navigate to `App Integrations` > `Azure Application` and click on `New Application`.

![IDmelon Panel](/images/vendor/app_integration/azure_application/idmelon_01.png)

Fill in the fields using the information from your application.

Then, click `Test Configuration` to verify the connection. If the test is successful, click Create.

![IDmelon Panel](/images/vendor/app_integration/azure_application/idmelon_02.png)
