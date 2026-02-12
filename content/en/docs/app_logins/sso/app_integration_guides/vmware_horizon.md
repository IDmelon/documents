---
title: "VMware Horizon"
description: "Integration Guide for IDmelon as an Identity Provider (IdP) with VMware Horizon."
lead: "This guide explains how to integrate IDmelon as an external IdP for VMware Horizon using SAML authentication."
date: 2025-02-17T17:58:15+03:30
lastmod: 2025-02-17T17:58:15+03:30
draft: false
images: []
menu:
  docs:
    parent: "idp_integration_guides"
weight: 72600
toc: true
---

## Overview

This guide provides step-by-step instructions for integrating `IDmelon` as an external Identity Provider (IdP) with `VMware Horizon` using SAML authentication.

![alt](/images/vendor/sso/vmware_horizon/horizon-saml-overview.png)

To achieve this integration, we will:

1. Create a SAML 2.0 configuration in the IDmelon Admin panel.
2. Configure `Unified Access Gateway (UAG)` and `Horizon Connection Server` in VMware.
3. Set up authentication settings in the VMware environment.

## Step 1: Create a SAML 2.0 IdP in IDmelon Panel

To configure IDmelon as an IdP for VMware Horizon, follow these steps:

1. Log in to the `IDmelon` panel.
2. Navigate to `App Integrations` → `Single Sign-On` → `New Application`.
3. Select `VMware Horizon` from the list of configurations.
4. In the `General Settings` section, configure the following fields:
   - **Entity ID:** `https://<HORIZON_UAG_URL>/portal`
   - **ACS URL:** `https://<HORIZON_UAG_URL>/portal/samlsso`
   - **Horizon UAG Metadata XML file:** Upload this file after configuring UAG and downloading the Metadata file from the UAG admin panel.
   - You need MetaURL(Idp Entity ID) and SAML Metadata of `IDP SAML Configuration` part for next steps.

![alt](/images/vendor/sso/vmware_horizon/idmelon_01.png)
![alt](/images/vendor/sso/vmware_horizon/idmelon_02.png)

## Step 2: Configure Horizon Server Connection

Navigate to the `Servers` section in the VMware Horizon settings.

   ![alt](/images/vendor/sso/vmware_horizon/vmware_01.png)

In the `Connection Servers` list, select a server and click `Edit`.

   ![alt](/images/vendor/sso/vmware_horizon/vmware_02.png)

### Create Identity Provider

In the `Authentication` tab:

- Set `Delegation of authentication to VMware Horizon (SAML 2.0 Authenticator)` to `Allow`.
- Click on `Manage SAML Authenticators` to add a new authentication provider.
![alt](/images/vendor/sso/vmware_horizon/vmware_03.png)

Click the `Add` button at the top.

   ![alt](/images/vendor/sso/vmware_horizon/vmware_04.png)

Choose `Static` Configuration.

   ![alt](/images/vendor/sso/vmware_horizon/vmware_05.png)

Paste the copied XML metadata into `SAML Metadata` and enter a name in `Label`.
Click `OK` to save changes.

   ![alt](/images/vendor/sso/vmware_horizon/vmware_06.png)

## Step 3: Connect Unified Access Gateway (UAG) to IDmelon

1. Log in to your UAG admin panel: `https://<HORIZON_UAG_URL>:9443/admin`.
2. Click `Select` under `Configure Manually`.

   ![alt](/images/vendor/sso/vmware_horizon/uag_01.png)
   ![alt](/images/vendor/sso/vmware_horizon/uag_02.png)

3. Scroll down to `Identity Bridging Settings` and click `Upload Identity Provider Metadata`.

   ![alt](/images/vendor/sso/vmware_horizon/uag_03.png)

4. Configure the following:
   - Set `Entity ID` to the value provided by IDmelon.
   - Download Metadata from the IDmelon Panel and upload it.
   - Click `Save`.

   ![alt](/images/vendor/sso/vmware_horizon/uag_04.png)

5. Click `SHOW` next to `Edge Service Settings`.

   ![alt](/images/vendor/sso/vmware_horizon/uag_05.png)

6. Click the gear icon next to `Horizon Settings`.

   ![alt](/images/vendor/sso/vmware_horizon/uag_06.png)

7. Click `More` at the bottom of the page.

   ![alt](/images/vendor/sso/vmware_horizon/uag_07.png)

8. Configure authentication settings:
   - Set `Auth Methods` to `SAML`.
   - In the `Identity Provider` drop-down, select the IDmelon provider.
   - Download the SAML service provider metadata and upload it to the IDmelon panel (SP SAML Configuration - Horizon UAG Metadata XML file).
   - Scroll down and click `Save`.

   ![alt](/images/vendor/sso/vmware_horizon/uag_08.png)

## Troubleshooting

You have successfully integrated IDmelon as an Identity Provider (IdP) with VMware Horizon. To troubleshoot any issues, you can download logs from the bottom of the UAG admin panel.
