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
This guide explains how to set up `IDmelon` as an external IdP for `Citrix`.

## Login to the Citrix Administration Panel

1. Click on “Manage Authentication Methods”.
  ![alt](/images/vendor/sso/citrix/citrix_SP_01.png)
2. Click on “SAML Authentication”.
  ![alt](/images/vendor/sso/citrix/citrix_SP_02.png)
3. Select the “SAML Authentication” option and choose “Service Provider”.
  ![alt](/images/vendor/sso/citrix/citrix_SP_03.png)
4. Copy the value of the “Service Provider Identifier” field. You will use this as the “Entity ID” when adding the app in your IDmelon Administration panel.
  ![alt](/images/vendor/sso/citrix/citrix_SP_04.png)
5. Click the “Browse” button next to “Export Encryption Certificate”:
  ![alt](/images/vendor/sso/citrix/citrix_SP_05.png)
  Save the certificate file with a custom name, such as `sp_enc.cer`.
  ![alt](/images/vendor/sso/citrix/citrix_SP_06.png)
6. Convert the exported certificate file from binary encoded `.cer` format to `.pem` format:

  ```shell
  openssl x509 -in sp_enc.cer -out sp_enc.pem -outform PEM
  ```

  The output file will be `sp_enc.pem`. You will use this file in the IDmelon Administration Panel.

## IDmelon SAML Configuration

1. Log in to the IDmelon Administration Panel and navigate to “App Integrations” > “Single Sign-on”. Then click the “New Application” button:
  ![alt](/images/vendor/sso/citrix/citrix_IDP_01.png)
2. Click the “Citrix” feature icon:
  ![alt](/images/vendor/sso/citrix/citrix_IDP_02.png)
3. On this page, you will need the “IdP Single Sign-on (Address)” and “Certificate” to configure the Identity Provider in Citrix in the next step.
  ![alt](/images/vendor/sso/citrix/citrix_IDP_03.png)
  At the bottom of the page, click “Upload Encryption Certificate File” and select `sp_enc.pem` to upload the Service Provider encryption public key. Also, fill in “Entity ID” with the “Service Provider Identifier” from step 4. The “Assertion Consumer Service URL” will be generated in the following step.
  ![alt](/images/vendor/sso/citrix/citrix_IDP_04.png)
    Sample Entity ID:

    ```
    https://srv8431835910.idmelon.ctx/Citrix/idmelonAuth
    ```

    Sample Assertion Consumer Service URL:

    ```
    https://srv8431835910.idmelon.ctx/Citrix/idmelonAuth/SamlForms/AssertionConsumerService
    ```
  
  4. To get the value for the “Assertion Consumer Service URL” field:
    In Windows `PowerShell ISE`, run:
    
      ```shell
      Get-STFStoreService | Out-String -Stream | Select-String "VirtualPath"
      ```
      ![alt](/images/vendor/sso/citrix/citrix_SP_07.png)
      
      Then run the following commands, replacing `/Citrix/Store` with the value you obtained above:
      
      ```shell
      Get-STFStoreService | Out-String -Stream | Select-String "VirtualPath"
      $storeVirtualPath = "/Citrix/Store"
      $auth = Get-STFAuthenticationService -Store (Get-STFStoreService -VirtualPath $storeVirtualPath)
      $acs = New-Object System.Uri $auth.Routing.HostbaseUrl, ($auth.VirtualPath + "/SamlForms/AssertionConsumerService")
      echo $acs
      ```

      ![alt](/images/vendor/sso/citrix/citrix_SP_08.png)

      Use the `AbsoluteUri` as the “Assertion Consumer Service URL” in the IDmelon configuration form.

5. Convert the “Certificate” file you downloaded to `.cer` format:

      ```shell
      openssl x509 -inform PEM -in cert.pem -outform DER -out idp.cer
      ```

    The output file will be `idp.cer`, which you will use in the Citrix Panel.

6. Return to “Manage Authentication Methods” in the Citrix Panel. Click on the “SAML Authentication” option and then on “Identity Provider”.
  ![alt](/images/vendor/sso/citrix/citrix_SP_09.png)
7. Set “SAML Binding” to “Post”.
  Set “Address” to the value copied from “IdP Single Sign-on URL (Address)” in step 3.
  ![alt](/images/vendor/sso/citrix/citrix_SP_10.png)
8. Click the “Import” button and select the `idp.cer` file you converted in step 5 for the “IdP Signature Certificate” section.
  ![alt](/images/vendor/sso/citrix/citrix_SP_11.png)

## Citrix XML Trust Configuration

If, after integration, you receive a `Cannot start desktop <Delivery Group Name>` error, you need to enable `TrustRequestsSentToTheXMLServicePort` in your Citrix StoreFront. This allows StoreFront to perform actions on behalf of the user without password validation.

- Use the Citrix Virtual Apps and Desktops PowerShell SDK to verify and configure XML trust:
  - Use `Get-BrokerSite` to check `TrustRequestsSentToTheXMLServicePort`.
  - Use `Set-BrokerSite -TrustRequestsSentToTheXMLServicePort $true` to enable it.
In this document, you are going to set up `IDmelon` as an external IdP to the `Citrix`.