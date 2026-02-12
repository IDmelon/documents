---
title: "Import Users from LDAP"
description: "Import users, groups, and devices from LDAP/Active Directory into the IDmelon panel"
lead: ""
date: 2023-09-20T13:34:58+03:30
lastmod: 2023-09-20T13:34:58+03:30
draft: false
images: []
menu:
  docs:
    parent: "add_or_import_users"
weight: 31400
toc: true
---

This guide explains how to import your required resources, including users, groups, and devices, into the IDmelon panel via LDAP.

## Creating and Obtaining a Token

1. Go to the IDmelon panel and click on **Settings** from the **Workspace** section.
2. On the **Settings** page, click on the **API Key Management** section and then click the **Create** button.
3. Choose a desired name for the token in the **Token Name** section and then click the **Next** option.
4. Copy the displayed token value.

## Downloading the Tool

1. In the IDmelon panel, go to the **App Integrations** menu and select the **LDAP** page.
2. On the displayed page, download both the **SyncStream.exe** and **config.json** files and place them in a folder.

## Configuring the Tool

1. Open the **config.json** file and change the following values according to your LDAP settings:
   - `LDAP_SERVER`
   - `LDAP_PORT`
   - `LDAP_BASE_DN`
   - `LDAP_USER`
   - `LDAP_PASSWORD`
2. Set the `AUTH_TOKEN` value according to the copied token value from the token creation step.

## Syncing with Filtered Groups

### Step 1: Configure the config.json File

Ensure that the **config.json** file is correctly configured with the necessary settings.

### Step 2: Check Connection to Active Directory

Verify the connection to Active Directory by using the `healthcheck` parameter.

``` powershell
SyncStream.exe healthcheck
```

### Step 3: Retrieve List of Groups

Get the list of groups by using the `group` parameter. The result will be saved to a file named **group_list.txt**.

``` powershell
SyncStream.exe group
```

### Step 4: Filter Groups for Syncing

1. Open the **group_list.txt** file to review the list of groups.
2. Based on this list, create and save the groups you want to sync in the **group_filter.txt** file.

### Step 5: Sync Filtered Groups

Run the sync process to synchronize users and devices from the groups defined in the **group_filter.txt** file.

``` powershell
SyncStream.exe sync
```

### On Premises

If you are using this tool in an on-premises environment, you need to modify the following values in the `config.json` file:

1. **API URLs Configuration:**
   - Change `API_GROUP_URL` to your server's group API endpoint
   - Change `API_DEVICE_URL` to your server's device API endpoint

2. **SSL Certificate Configuration:**
   - If you are using self-signed certificates, place the certificate file named `cert.crt` next to the executable file (`SyncStream.exe`)
   - The tool will automatically detect and use this certificate for SSL verification

### Quick Commands Reference

| Action | Command |
|--------|---------|
| Edit Configuration | Edit `config.json` |
| Health Check | `SyncStream.exe healthcheck` |
| Retrieve Groups | `SyncStream.exe group` |
| Edit Group Filter | Edit the `group_filter.txt` file based on the `group_list.txt` file |
| Sync Groups | `SyncStream.exe sync` |
| Check Version | `SyncStream.exe version` |
| Generate Logs | `SyncStream.exe sync --dump` |
| Verbose Logging | `SyncStream.exe sync --log info` |

## Troubleshooting

- **Run Health Check**: Ensure this tool can communicate with Active Directory and the remote API:

  ``` powershell
  SyncStream.exe healthcheck
  ```

- **Add Debug Logging**: Add `--dump` to each command you run to save logs to a log file:

  ``` powershell
  SyncStream.exe sync --dump
  ```

- **Add Verbose Logging**: Add `--log info` to each command you run to see more detailed information:

  ``` powershell
  SyncStream.exe sync --log info
  ```
