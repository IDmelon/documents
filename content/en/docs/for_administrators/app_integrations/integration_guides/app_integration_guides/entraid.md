---
title: "Microsoft Entra ID"
description: ""
lead: ""
date: 2023-10-11T00:58:07+03:30
lastmod: 2023-10-11T00:58:07+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72100
toc: true
---

In this document you are going to set up `IDmelon` as an external IdP to the `EntraID`.

## IDmelon Configuration as IDP

Log in to the IDmelon panel, navigate to the **App Integration > Single Sign-On** section, and click **New Application**:

![IDmelon 1](/images/vendor/sso/entra_id_sp/entra_id_idmelon_1.png)

Then select `Okta` as service provider:

![IDmelon 2](/images/vendor/sso/entra_id_sp/entra_id_idmelon_2.png)

You will need the values provided in the newly opened window to set up `EntraID` as SP:

![IDmelon 3](/images/vendor/sso/entra_id_sp/entra_id_idmelon_3.png)

---

## Configuring EntraID as SP

The following commands need to be executed in **Windows PowerShell**:

---

`$cred = Get-Credential`

`Connect-MsolService -Credential $cred`

`Get-MsolDomain`

> Get all values for `{..}` from your IDmelon panel.
> If you are currently login here, you will see the replaced values instead.

```powershell
$dom = domain

$uri = idp_issuer_uri

$url = idp_single_sign_on_ur

$logouturl = idp_single_sign_on_url

$cert = idp_certificate_download_url
```

```powershell
Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $dom -Authentication Federated
-PassiveLogOnUri $url -SigningCertificate $cert -IssuerUri $uri -LogOffUri $logouturl
-PreferredAuthenticationProtocol SAMLP
```

Check your SAML configuration

**the result is :**

```powershell
$dom = domain
Get-MSolDomainFederationSettings -DomainName $dom | Format-List *
```

### You need to Set ImmutableID for your current user

---

Example of users.csv file:

UserPrincipalName

```csv
sample@vancosys.com;
example@vancosys.com;
```

### Load CSV

---

```powershell
$csvFile = Import-Csv C:\\idmelon\\users.csv -Delimiter ";"
```

### Create arrays for skipped and failed users

```powershell
$SkippedUsers = @()
$FailedUsers = @()
```

### Loop through CSV records

---

```powershell
    foreach ($item in $csvFile) {
        $upn = $item.UserPrincipalName
        $UserPrincipalName =  (Get-MsolUser -UserPrincipalName  $upn  | select UserPrincipalName).UserPrincipalName
        $objectID = (Get-MsolUser -UserPrincipalName  $upn  | select ObjectId).ObjectId.Guid
        if ($UserPrincipalName) {
            try{
            Set-MSOLuser -UserPrincipalName $UserPrincipalName -ImmutableID $objectID
            } catch {
            $FailedUsers += $upn
            Write-Warning "$upn user found, but FAILED to update."
            }
        }
        else {
            Write-Warning "$upn not found, skipped"
            $SkippedUsers += $upn
        }
    }
    foo()
```

### Show result

---

```powershell
Get-MsolUser -all | Select-Object UserprincipalName,objectID,ImmutableID
```

## Passwordless

---

- **Open** Azure directory admin.

![alt](/images/vendor/sso/entra_id_sp/office_passless.png)

- **Click on** users.

![alt](/images/vendor/sso/entra_id_sp/office_passless1.png)

- **Choose** a user.

- **Click on** Authentication methods.

- **Delete** Authentication method option.

![alt](/images/vendor/sso/entra_id_sp/office_passless2.png)

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
