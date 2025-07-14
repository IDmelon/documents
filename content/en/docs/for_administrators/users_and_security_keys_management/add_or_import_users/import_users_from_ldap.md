---
title: "Import Users from LDAP"
description: ""
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

This guide is for importing your required resources,
including users, groups, and devices, into the IDmelon panel via LDAP.

### Creating and Obtaining a Token

1. Go to the IDmelon panel and then click on `Settings` from the `Workspace` section.
2. On the `Settings` page, click on the `API Key Management` section and then click on the `Create` button.
3. Choose a desired name for the token in the `Token Name` section and then click on the `Next` option.
4. Copy the displayed token value.

### Downloading the Tool

1. In the IDmelon panel, go to the `Users` menu and then `All Users`. Click on the `Import Users` option and select `LDAP` from the dropdown menu.
2. On the displayed page, download both the `SyncStream.exe` and `config.json` files and place them in a folder.

### Configuring the Tool

1. Open the `config.json` file and change the following values according to your LDAP settings:
   - `LDAP_SERVER`
   - `LDAP_PORT`
   - `LDAP_BASE_DN`
   - `LDAP_USER`
   - `LDAP_PASSWORD`
2. Set the `AUTH_TOKEN` value according to the copied token value from the token creation and obtaining step.

### Syncing with filtered groups

1. Configure the `config.json` File
   - Ensure that the `config.json` file is correctly configured with the necessary settings.

2. Check Connection to Active Directory
   - Verify the connection to Active Directory by using the `healthcheck` parameter.
   - Command: `SyncStream.exe healthcheck`

3. Retrieve List of Groups
   - Get the list of groups by using the `group` parameter. The result will be saved to a file named `group_list.txt`.
   - Command: `SyncStream.exe group`

4. Filter Groups for Syncing
   - Open the `group_list.txt` file to review the list of groups.
   - Based on this list, create and save the groups you want to sync in the `group_filter.txt` file.

5. Sync Filtered Groups
   - Run the sync process to synchronize users and devices from the groups defined in the `group_filter.txt` file.
   - Command: `SyncStream.exe sync`

### On Premises

If you are using this tool in an on-premises environment, you need to modify the following values in the `config.json` file:

1. **API URLs Configuration:**
   - Change `API_GROUP_URL` to your server's group API endpoint
   - Change `API_DEVICE_URL` to your server's device API endpoint

2. **SSL Certificate Configuration:**
   - If you are using self-signed certificates, place the certificate file named `cert.crt` next to the executable file (`SyncStream.exe`)
   - The tool will automatically detect and use this certificate for SSL verification

### Quick Commands Reference

1. Edit Configuration:
   - Edit `config.json`

2. Health Check:
   - `SyncStream.exe healthcheck`

3. Retrieve Groups:
   - `SyncStream.exe group`

4. Edit Group Filter:
   - Edit the `group_filter.txt` file based on the `group_list.txt` file.

5. Sync Groups:
   - `SyncStream.exe sync`

6. Check version:
   - `SyncStream.exe version`

7. Log:
   - `SyncStream.exe sync --dump`
   - `SyncStream.exe sync --log info`

### Troubleshooting

- Run Health Check option to ensure this tool can communicate with AD and the remote API:
  - `SyncStream.exe healthcheck`
- Add `--dump` to each command you run to save logs to a log file.
  - `SyncStream.exe sync --dump`
- Add `--log info` to each command you run to see more info.
  - `SyncStream.exe sync --log info`
