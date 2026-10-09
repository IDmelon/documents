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
2. On the **Authentication** page, click on the **API Key Management** section and then click the **New API Key** button.
3. Choose a desired name for the token in the **Name** section, select the key type as **Admin**, and then click the **Next** option.
4. Copy the displayed token value.

## Downloading the Tool

1. In the IDmelon panel, go to the **App Integrations** menu and select the **LDAP** page.
2. On the displayed page, download both the **SyncStream.exe** and **config.json** files and place them in a folder.

## Configuring the Tool

1. Open the **config.json** file and change the following values according to your LDAP settings:

   | Parameter | Description | Example |
   |-----------|-------------|---------|
   | `LDAP_SERVER` | Hostname (FQDN) or URL of your LDAP / Active Directory server | `ad.company.com` or `ldaps://ad.company.com` |
   | `LDAP_PORT` | `389` for plain LDAP, `636` for LDAPS (LDAP over SSL/TLS) | `389` or `636` |
   | `LDAP_BASE_DN` | Distinguished Name of the subtree that contains the groups you want to sync | `dc=company,dc=com` |
   | `LDAP_USER` | An account with read access to the directory, either in UPN form or as a full DN | `sync@company.com` |
   | `LDAP_PASSWORD` | Password of the account above | `YOUR-PASSWORD` |
   | `LDAP_PLATFORM` | `AD` for Active Directory, `OA` for OpenLDAP | `AD` |

2. Set the `AUTH_TOKEN` value according to the copied token value from the token creation step.

> **Note** Windows Server 2025 domain controllers require LDAP signing and encryption by default, which means connections on the unencrypted port 389 are rejected. In this case, specify the LDAP server using the `ldaps://` format and set the port to `636`:
>
> ``` json
> "LDAP_SERVER": "ldaps://ad.company.com",
> "LDAP_PORT": 636,
> ```
>
> Always use the fully qualified domain name (FQDN) of the domain controller exactly as it appears in its certificate, not an IP address or short name.
>
> If your Active Directory certificate is issued by Active Directory Certificate Services (AD CS) or another internal certificate authority, run SyncStream from a domain-joined workstation or server that trusts the issuing root certificate.
>
> Earlier Windows Server versions do not require this by default, but your administrator may have enabled the same requirement through Group Policy.

### Choosing Between Port 389 and Port 636

If you are not sure which port your directory accepts, check which one is listening before you run a sync:

``` powershell
Test-NetConnection ad.company.com -Port 636
Test-NetConnection ad.company.com -Port 389
```

A result of `TcpTestSucceeded : True` means the port is reachable.

- If port **636** is reachable, prefer it. Set `LDAP_SERVER` to `ldaps://<fqdn>` and `LDAP_PORT` to `636`.
- Use port **389** only if LDAPS is not available in your environment. If the bind is rejected on port 389, your domain controller requires signing or encryption and you must switch to LDAPS.

You can also confirm LDAPS is published correctly with **ldp.exe** on a domain-joined machine: open **Connection** > **Connect**, enter the domain controller FQDN, set the port to `636`, select **SSL**, and connect.

> **Note** Do not combine `ldaps://` with port `389` (or a plain hostname with port `636`). The scheme and the port must match, otherwise the connection fails during the TLS handshake.

### Example config.json

``` json
{
  "LDAP_SERVER": "ldaps://ad.company.com",
  "LDAP_PORT": 636,
  "LDAP_BASE_DN": "dc=company,dc=com",
  "LDAP_USER": "sync@company.com",
  "LDAP_PASSWORD": "YOUR-PASSWORD",
  "LDAP_PLATFORM": "AD",
  "AUTH_TOKEN": "YOUR-TOKEN",
  "API_GROUP_URL": "https://skm.idmelon.com/administrator/ad/sync",
  "CHUNK_SIZE": 10
}
```

For an environment without LDAPS, use `"LDAP_SERVER": "ad.company.com"` and `"LDAP_PORT": 389` instead.

## Syncing with Filtered Groups

### Step 1: Configure the config.json File

Ensure that the **config.json** file is correctly configured with the necessary settings.

### Step 2: Check Connection to Active Directory

Verify the connection by using the `healthcheck` parameter. This command checks two things: that SyncStream can bind to your LDAP server with the credentials in **config.json**, and that it can reach the API address in `API_GROUP_URL`.

``` powershell
SyncStream.exe healthcheck
```

Run this command after every change to **config.json**. It is the fastest way to confirm that your server address, port, and credentials are correct before you try to sync. If it reports an error, see the [Troubleshooting](#troubleshooting) section below.

### Step 3: Retrieve List of Groups

Get the list of groups by using the `group` parameter. The result will be saved to a file named **group_list.txt**.

``` powershell
SyncStream.exe group
```

### Step 4: Filter Groups for Syncing

1. Open the **group_list.txt** file to review the list of groups.
2. Create a **group_filter.txt** file next to **SyncStream.exe** and copy into it the lines of the groups you want to sync.

Copy the lines from **group_list.txt** exactly as they are. Each line must keep the `GroupName : DN` format, including the spaces around the colon:

``` text
IT Department : CN=IT Department,OU=Groups,DC=company,DC=com
HR Team : CN=HR Team,OU=Groups,DC=company,DC=com
```

> **Note** Nested groups are not expanded. If a group in your filter contains another group, the members of that nested group are not synced unless you also add the nested group on its own line.

### Step 5: Sync Filtered Groups

Run the sync process to synchronize users and devices from the groups defined in the **group_filter.txt** file.

``` powershell
SyncStream.exe sync
```

### On Premises

If you are using this tool in an on-premises environment, you need to modify the following values in the `config.json` file:

1. **API URL Configuration:**
   - Change `API_GROUP_URL` to your server's sync API endpoint

2. **SSL Certificate Configuration:**
   - If your server uses a self-signed or internally issued certificate, place the certificate file named `cert.crt` next to the executable file (`SyncStream.exe`)
   - The tool automatically detects this certificate and uses it to verify the API connection
   - The file must be in PEM format and should contain the certificate of the issuing certificate authority

> **Note** The `cert.crt` file and the `--disable-ssl-verify` option apply only to the connection to the API server. They have no effect on the connection to your LDAP server. For LDAP certificate issues, use `--ssl-debug-mode` instead, as described in the [SSL/TLS Issues](#ssltls-issues) section.

If your on-premises API endpoint does not use TLS at all, set `API_GROUP_URL` to an `http://` address. Certificate verification is then not performed and no certificate file is needed.

### Quick Commands Reference

| Action | Command |
|--------|---------|
| Edit Configuration | Edit `config.json` |
| Health Check | `SyncStream.exe healthcheck` |
| Retrieve Groups | `SyncStream.exe group` |
| Edit Group Filter | Edit the `group_filter.txt` file based on the `group_list.txt` file |
| Sync Groups | `SyncStream.exe sync` |
| Check Version | `SyncStream.exe version` |
| Export to a File Instead of Syncing | `SyncStream.exe export` |
| Generate Logs | `SyncStream.exe sync --dump` |
| Verbose Logging | `SyncStream.exe sync --log info` |
| Skip API Certificate Verification | `SyncStream.exe sync --disable-ssl-verify` |
| Force SSL to the LDAP Server and Skip Its Certificate Check | `SyncStream.exe healthcheck --ssl-debug-mode` |

The `--dump`, `--log`, `--disable-ssl-verify`, and `--ssl-debug-mode` options can be added to any command, and can be combined:

``` powershell
SyncStream.exe healthcheck --log info --dump
```

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

  A **log.txt** file is created next to **SyncStream.exe**. Include it when you contact support.

### Common Issues

| Symptom | Likely cause | What to do |
|---------|--------------|------------|
| `Ldap connection failed` with `invalidCredentials` or `data 52e` | Wrong username or password | Check `LDAP_USER` and `LDAP_PASSWORD`. Try the UPN form (`user@company.com`) or the full DN of the account |
| The bind is rejected on port `389`, or the error mentions `strongerAuthRequired` or `confidentiality required` | The domain controller requires a signed or encrypted connection, which is the default on Windows Server 2025 | Switch to `ldaps://<fqdn>` with port `636` |
| The connection to port `636` times out or is refused | LDAPS is not enabled on the domain controller, or a firewall is blocking the port | Confirm with `Test-NetConnection <fqdn> -Port 636`. The domain controller needs a valid server certificate for LDAPS |
| `CERTIFICATE_VERIFY_FAILED` while running `sync` | The API server certificate is not trusted by the machine running SyncStream | See [SSL/TLS Issues](#ssltls-issues) below |
| **group_list.txt** is empty or missing groups | `LDAP_BASE_DN` points to a subtree that does not contain the groups, or the account lacks read permission | Set `LDAP_BASE_DN` to a higher level, for example `dc=company,dc=com` |
| `Group filter file not found` or `Invalid format at line X` | **group_filter.txt** is missing, or a line is not in the `GroupName : DN` format | Place the file next to **SyncStream.exe** and copy the lines from **group_list.txt** without editing them |
| Some users or devices are missing after a sync | They belong only to a nested group, or they are not direct members of a filtered group | Add every group you want to sync to **group_filter.txt** on its own line |

### SSL/TLS Issues

SyncStream makes two separate secure connections, and each one is handled by a different option.

**Connection to the LDAP server**

If you suspect a certificate or TLS problem with your domain controller, run a health check with `--ssl-debug-mode`. This forces SyncStream to connect to the LDAP server over SSL/TLS and skips validation of the server certificate:

``` powershell
SyncStream.exe healthcheck --ssl-debug-mode --log info
```

Use this option together with `"LDAP_PORT": 636`, because it always connects over SSL/TLS. If the health check succeeds with `--ssl-debug-mode` but fails without it, the problem is with the certificate of the domain controller or with the trust chain on the machine running SyncStream. Install the root certificate of the issuing authority on that machine, or run SyncStream from a domain-joined machine that already trusts it.

> **Note** `--ssl-debug-mode` is intended for diagnosis only. Do not use it as a permanent configuration.

**Connection to the API server**

If the IDmelon API is reached over HTTPS with a certificate that the machine does not trust, which usually happens in on-premises deployments, place the certificate of the issuing authority in a file named `cert.crt` next to **SyncStream.exe**. SyncStream detects and uses it automatically.

As a last resort, you can skip certificate verification for the API connection:

``` powershell
SyncStream.exe sync --disable-ssl-verify
```

> **Note** `--disable-ssl-verify` turns off certificate verification for the API connection and is not recommended outside of testing. Prefer the `cert.crt` file.

No certificate configuration is needed when you sync with the IDmelon cloud at `https://skm.idmelon.com`.
