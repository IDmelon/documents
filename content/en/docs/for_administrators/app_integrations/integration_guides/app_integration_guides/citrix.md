---
title: "Citrix"
description: "Integrate Citrix with IDmelon"
lead: ""
date: 2023-12-15T23:05:00+03:30
lastmod: 2025-06-08T23:05:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "simple_signon_and_idp_integration"
weight: 25
toc: true
---

In this document, you are going to set up `IDmelon` as an external IdP to the `Citrix`.

## Login to Citrix administration panel

1. Click on the “Manage Authentication Methods”
  ![alt](/images/vendor/sso/citrix/citrix_SP_01.png)
2. Click on the “SAML Authentication”
  ![alt](/images/vendor/sso/citrix/citrix_SP_02.png)
3. Click on the “SAML Authentication” option and choose on the “Service Provider”
  ![alt](/images/vendor/sso/citrix/citrix_SP_03.png)
4. Copy the value of “Service Provider Identifier” field for future use when adding the app into your IDmelon Orchestrate panel. this field will used as “Entity Id”.
  ![alt](/images/vendor/sso/citrix/citrix_SP_04.png)
5. Click on the “Browse” button of “Export Encryption Certificate”:
  ![alt](/images/vendor/sso/citrix/citrix_SP_05.png)
  And save this certificate file with custom name like `sp_enc.cer`.
  ![alt](/images/vendor/sso/citrix/citrix_SP_06.png)
6. Convert this exported certificate file from the binary encoded cer format to the  pem format:

  ```shell
  openssl x509 -in sp_enc.cer -out sp_enc.pem -outform PEM
  ```

  The output file will be `sp_enc.pem`.
  We will use this file in IDmelon Orchestrate Panel.

## IDmelon SAML configuration

1. Login to the IDmelon Orchestrate Panel and navigate to the “App Integrations" and “Single Sign-on"
Then Click on “New Application” button:
  ![alt](/images/vendor/sso/citrix/citrix_IDP_01.png)
2. Click on the “Create a custom configuration” button:
  ![alt](/images/vendor/sso/citrix/citrix_IDP_02.png)
3. Set a custom name “Citrix” and click next.
  ![alt](/images/vendor/sso/citrix/citrix_IDP_03.png)
4. Fill the all fields as describe in the picture.
  Set the “Entity ID” with value that you copied It as “Service Provider Identifier” in the step 4 of the Login to Citrix administration panel section .
  ![alt](/images/vendor/sso/citrix/citrix_IDP_04.png)
  ![alt](/images/vendor/sso/citrix/citrix_IDP_04_01.png)

    Sample Entity ID:

      http://srv8431835910.idmelon.ctx/Citrix/idmelonAuth

    Sample ACS:

      https://srv8431835910.idmelon.ctx/Citrix/idmelonAuth/SamlForms/AssertionConsumerService

5. To get the value for the “Assertion Consumer Service” field:

    In the Windows PowerShell ISE run this command:

    ```shell
    Get-STFStoreService | Out-String -Stream | Select-String  "VirtualPath"
    ```

    ![alt](/images/vendor/sso/citrix/citrix_SP_07.png)

    Run the below commands and remember to change the value of `/Citrix/Store` with the value you obtained in the previous step.

    ```shell
    Get-STFStoreService | Out-String -Stream | Select-String  "VirtualPath"
    $storeVirtualPath = "/Citrix/Store"
    $auth = Get-STFAuthenticationService -Store (Get-STFStoreService -VirtualPath $storeVirtualPath)
    $acs = New-Object System.Uri $auth.Routing.HostbaseUrl, ($auth.VirtualPath + "/SamlForms/AssertionConsumerService")
    echo $acs
    ```

    ![alt](/images/vendor/sso/citrix/citrix_SP_08.png)

    Use `AbsoluteUri` as “Assertion Consumer Service” for IDmelon configuration form.

6. For the “Signing Certificate” field, click “Upload Certificate File” and select the converted sp_enc.pem file in the step 5 of the Login to Citrix administration panel.
![alt](/images/vendor/sso/citrix/citrix_IDP_05.png)
7. In the next page define all attributes as picture and click on the confirm button:
  ![alt](/images/vendor/sso/citrix/citrix_IDP_06.png)

    | SP variable name    | IDP variable name |
    |---------------------|-------------------|
    | email               | EMAIL             |
    | username            | EMAIL             |
    | userprincipalname   | EMAIL             |

8. In the IDmelon Panel , Form the “App Integrations” menu, navigate to the “Single Sing-on” menu and click on the “Edit” icon of created “Citrix” application:
  ![alt](/images/vendor/sso/citrix/citrix_IDP_07.png)
9. Copy the value of “Idp Entity ID”. We will use in the Citrix Panel.
  ![alt](/images/vendor/sso/citrix/citrix_IDP_08.png)
10. Download “IdP Signature Certificate” file and convert it to the cer format:

    ```shell
    openssl x509 -inform PEM -in cert.pem -outform DER -out idp.cer
    ```

    We will use in the Citrix Panel.
    The output file will be `idp.cer`.
11. Back to the “Manage Authentication Methods” of Citrix Panel
  Click on the “SAML Authentication” option and click on the “Identity Provider”
  ![alt](/images/vendor/sso/citrix/citrix_SP_09.png)
12. Set the value of “SAML Binding” to “Post”.
Set the value of “Address” to the value copied from “IdP Single Sign-on URL” in the step 9.
  ![alt](/images/vendor/sso/citrix/citrix_SP_10.png)
13. Click on the “Import” button and select the idp.cer file that is converted in the “IdP Signature Certificate” section of the step 10.
  ![alt](/images/vendor/sso/citrix/citrix_SP_12.png)
