---
title: "Microsoft Entra ID"
description: ""
lead: ""
date: 2023-10-11T00:58:07+03:30
lastmod: 2025-11-03T13:24:07+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72100
toc: true
---

This document guides you through setting up `IDmelon` as an external IdP for `Entra ID`.

## IDmelon Configuration as IDP

Log in to the IDmelon panel, navigate to the **App Integration > Single Sign-On** section, and click **New Application**:

![IDmelon 1](/images/vendor/sso/entra_id_sp/entra_id_idmelon_1.png)

Then select `Entra ID` as service provider:

![IDmelon 2](/images/vendor/sso/entra_id_sp/entra_id_idmelon_2.png)

You will need the values provided in the newly opened window to set up `EntraID` as SP:

![IDmelon 3](/images/vendor/sso/entra_id_sp/entra_id_idmelon_3.png)

---

> **Note:** All following commands are required to run in **Windows PowerShell**.


## Configuring EntraID as SP

To configure `Entra ID`, you need to use the `Connect-MgGraph` command. If it's not installed on your system, use the following instruction as an `Administrator` user:

```powershell
Install-Module Microsoft.Graph -Scope CurrentUser
```

Connect to Microsoft Graph using the following command. This command will attempt to sign in to Azure. The first time you run it, a dialog will appear showing a warning containing Microsoft links. Add all these links to the Windows trust list, then follow the login steps to complete the connection.

```powershell
Connect-MgGraph -Scopes "Domain.ReadWrite.All","User.ReadWrite.All","Directory.Read.All"
```

Check domains and authentication type using the following command: 
```powershell
Get-MgDomain | Select-Object Id, IsVerified, IsDefault, AuthenticationType
```

If the domain's AuthenticationType is "Managed", you don't have an active federation record and should create one. If it is "Federated", you should update the existing record. For this purpose, first create an IdP signature in Base64 format. IDmelon provides a `pem` file, so you can convert it to Base64 format using the following instructions:

```powershell
# Assuming your .pem file is saved at this path
$certPath = "C:\Path\To\PEM_File\idp_certificate_sample.pem" 

# Read the file content, filter out BEGIN/END lines, and join the remaining lines into a single string
$certBase64 = (Get-Content $certPath | Where-Object { 
    $_ -notlike "-----BEGIN CERTIFICATE-----" -and 
    $_ -notlike "-----END CERTIFICATE-----" 
}) -join ""

# Display the result to ensure it's a single line
Write-Host "Base64 String: $($certBase64)" 
```

### Create Federation Record

To create a federation configuration, execute the following script in **Windows PowerShell**. All variables should be set with valid data to run successfully.

```powershell
$dom        = "yourdomain.com"   # verified domain
$uri        = "https://skm.idmelon.com/api/saml/idp/metadata/sample"    # example
$url        = "https://skm.idmelon.com/api/saml/idp/login/sample"       # SAML endpoint for login
$logouturl  = "https://panel.idmelon.com/auth/logout?state=logout"      # logout endpoint
$certBase64 = "signiture-sample"   # your IdP signing cert content

# required extra parameters for Graph
$protocol   = "saml"
$mfaMode    = "acceptIfMfaDoneByFederatedIdp"   # required, see below

New-MgDomainFederationConfiguration -DomainId $dom `
  -DisplayName $dom `
  -IssuerUri $uri `
  -PassiveSignInUri $url `
  -SignOutUri $logouturl `
  -SigningCertificate $certBase64 `
  -PreferredAuthenticationProtocol $protocol `
  -FederatedIdpMfaBehavior $mfaMode `
  -IsSignedAuthenticationRequestRequired:$false
```

### Update Federation Record

If your authentication type is "Federated" and you need to update the record, execute the following script. All variables should be set with valid data to run successfully.

```powershell
$dom        = "yourdomain.com"   # verified domain
$uri        = "https://skm.idmelon.com/api/saml/idp/metadata/sample"    # example
$url        = "https://skm.idmelon.com/api/saml/idp/login/sample"       # SAML endpoint for login
$logouturl  = "https://panel.idmelon.com/auth/logout?state=logout"      # logout endpoint
$certBase64 = "signiture-sample"   # your IdP signing cert content

$fed = Get-MgDomainFederationConfiguration -DomainId $dom | Select-Object -First 1
Update-MgDomainFederationConfiguration -DomainId $dom -InternalDomainFederationId $fed.Id `
  -IssuerUri $uri `
  -PassiveSignInUri $url `
  -SignOutUri $logouturl `
  -SigningCertificate $certBase64 `
```

### Verify Configuration

Now verify the change by executing the following command:
```powershell
Get-MgDomain -DomainId $dom | Select-Object Id, AuthenticationType, IsVerified
```
You should now see:
```sql
Id            AuthenticationType IsVerified
------------- ------------------ ----------
yourdomain.com Federated         True
```
You can also confirm the new configuration:

```powershell
Get-MgDomainFederationConfiguration -DomainId $dom | Format-List DisplayName,IssuerUri,PassiveSignInUri,PreferredAuthenticationProtocol
```

### Show All Federated Users

If you want to see all users for whom federation is set, execute the following script:

---

```powershell
# Step 1: Connect to Microsoft Graph with the required scopes
Connect-MgGraph -Scopes "User.Read.All", "Directory.Read.All"

# Step 2: Retrieve all users and filter those with a non-null OnPremisesImmutableId
$users = Get-MgUser -All -Property UserPrincipalName, OnPremisesImmutableId

# Step 3: Filter users who have OnPremisesImmutableId set (Federated users)
$federatedUsers = $users | Where-Object { $_.OnPremisesImmutableId }

# Step 4: Print the list of federated users
$federatedUsers | Select-Object UserPrincipalName, OnPremisesImmutableId | Format-Table -AutoSize
```

### Revert Federation to Managed Authentication

To revert federation and switch the domain back to "Managed" authentication, you need to remove the federation configuration. First, retrieve the federation configuration for the domain:

```powershell
$dom = "yourdomain.com"   # Replace with your federated domain

# Get the federation configuration for the domain
$federationConfig = Get-MgDomainFederationConfiguration -DomainId $dom | Select-Object -First 1
```

Then remove the domain federation configuration to revert to Managed authentication:

```powershell
# Remove the domain federation configuration to revert to Managed authentication
Remove-MgDomainFederationConfiguration -DomainId $dom -InternalDomainFederationId $federationConfig.Id
```

Verify that the domain has been reverted to Managed authentication:

```powershell
Get-MgDomain -DomainId $dom | Select-Object Id, AuthenticationType, IsVerified
```

You should now see:
```sql
Id            AuthenticationType IsVerified
------------- ------------------ ----------
yourdomain.com Managed          True
```

## Passwordless Configuration

To configure passwordless authentication:

1. **Open** Azure directory admin.

![Azure Directory Admin](/images/vendor/sso/entra_id_sp/office_passless.png)

2. **Click on** Users.

![Users Menu](/images/vendor/sso/entra_id_sp/office_passless1.png)

3. **Choose** a user.

4. **Click on** Authentication methods.

5. **Delete** the Authentication method option.

![Authentication Methods](/images/vendor/sso/entra_id_sp/office_passless2.png)

<!-- ## API Token

From the side menu, navigate to the `App registrations` menu and click `New registrations`.

![alt](/images/vendor/sso/entra_id_sp/office_token_01.png)

- Enter the desired name for your Application.

- Then from `Supported account types` select who can use your API.

  - **Tip :** for more information of the choices please select **Help me Choose....**

- Click Register.

![alt](/images/vendor/sso/entra_id_sp/office_token_02.png)

Then you can see your API information, which you have Copy and send these informations to idmelon.

- Copy **Application (client) ID**.

- Copy **Directory (tenant) ID**.

Then please for accessing to Client Secret click Add a certificate or secret in Client credentials field.

![alt](/images/vendor/sso/entra_id_sp/office_token_03.png)

From **Certificate & secrets** panel please click on **New client secret** :

- Enter the desired description for your client secret.

- Click `Add`.

![alt](/images/vendor/sso/entra_id_sp/office_token_04.png)

Finally, you can Copy your **client secret** from Client credentials field.

![alt](/images/vendor/sso/entra_id_sp/office_token_05.png)

> **Please send these information to IDmelon, in order to get your users from office panel.** -->
