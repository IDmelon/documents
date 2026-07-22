---
title: "Passkey Provisioner Tool"
description: ""
lead: ""
date: 2025-04-15T15:00:00+03:30
lastmod: 2026-07-08T15:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "manage_passkeys_and_credentials"
weight: 34090
toc: true
---

The **IDmelon Passkey Provisioner** is a Windows desktop application (CLI) that writes FIDO2 passkeys onto physical security keys for your users. It works together with a **provisioning workflow** created in the IDmelon Admin Panel and can operate in two modes:

- **Admin-Managed** — an administrator provisions passkeys on behalf of users.
- **Self-Service** — users provision their own passkey.

## Creating a provisioning workflow in the Admin Panel

Both modes start from a provisioning workflow (request) created in the Admin Panel.

1. Log in to the [IDmelon Admin Panel](https://panel.idmelon.com).
2. Navigate to **Provisioning** and click **New Request**.

   ![Provisioning - New Request](/images/vendor/passkey_provisioner/panel_provisioning.png)

3. Select the users or groups to provision, and click **Next**.

4. Select the hardware type — for example, **Hardware → Crescendo Card** — and click **Next**.

5. Add the **Microsoft Entra ID** for the Integrated RPs, and click **Next**.

6. Choose the provisioning mode:
   - **Admin Managed** — you provision on behalf of users.
   - **Self-Service** — users provision their own passkeys.

   Then click **Next**.

7. Set a PIN — **random** or **static** — and click **Next**.

8. Click **Create Request**, and then **Provision Now** (Set the Reset checkbox to checked if the security key must be reset before provisioning).

9. Start provisioning:
   - **Admin-Managed:** select **Open passkey-provisioner** when the browser alert appears. The CLI launches, polls the workflow, and provisions each pending user with the connected security key.
   - **Self-Service:** each selected user receives an email with a provisioning link. The user opens the link, follows the on-screen steps, and selects **Open passkey-provisioner** when the alert appears.

> **Note:** When a run completes, each user's **status** is updated in the Admin Panel. Close the current workflow and start a new one for the next batch.

## Admin-Managed mode

In Admin-Managed mode you register an Admin API Key on the machine once, then provision users from the workflow.

### 1. Generate an Admin API Key

In the [IDmelon Admin Panel](https://panel.idmelon.com), go to **Authentication → API Key Management → New API Key** and create a key with **Type: Admin**. Copy the generated key.

![API Key Management - New Admin API Key](/images/vendor/passkey_provisioner/panel_api_key.png)

### 2. Register an IDmelon provider

Register the API key locally as an IDmelon provider.

Open PoweShell (Terminal) and enter the command below:

```bash
passkey-provisioner providers --add "Contoso"
```

Replace `Contoso` with a label for the provider (for example, your tenant name), then follow the interactive prompts:

- **Select a provider:** `1` (IDMELON)
- **Enter the API Key:** paste the Admin API Key from step 1
- **Enter the IDmelon endpoint:** the base URL (default `https://skm.idmelon.com`)

Activate it (only needed if you have more than one provider):

```bash
passkey-provisioner providers --activate "Contoso"
```

### 3. Create and run the workflow

Follow [Creating a provisioning workflow](#creating-a-provisioning-workflow-in-the-admin-panel) above, selecting **Admin Managed** in step 6. When you click **Open passkey-provisioner**, the CLI launches automatically and provisions each pending user. Insert/tap the security key when prompted, and remove it when the CLI asks.

## Self-Service mode

Self-Service mode needs **no API key and no provider** — steps 1 and 2 of Admin-Managed do **not** apply. The user authenticates with their own security key to obtain an access token, then provisions their own passkey.

1. An administrator creates the workflow (see [Creating a provisioning workflow](#creating-a-provisioning-workflow-in-the-admin-panel)), selecting **Self-Service** in step 6.
2. The user opens the provisioning link sent to their email address.
3. The user follows the on-screen steps and selects **Open passkey-provisioner** when the alert appears.
4. When provisioning starts, the CLI first prompts the user to **touch their security key** to obtain an access token, then provisions the passkey.

## CLI command reference

The CLI executable is `passkey-provisioner`. Supported commands: `--help`, `--version`, `profiles`, `providers`, `device`, `config`, and `create`.

Run any command with `--help` for command-specific guidance:

```bash
passkey-provisioner --help
passkey-provisioner profiles --help
passkey-provisioner providers --help
passkey-provisioner device --help
passkey-provisioner config --help
passkey-provisioner create --help
```

```bash
passkey-provisioner --version
```

### `profiles`

Manage provisioning profiles stored in the TOML configuration file. Exactly one option at a time.
> Note: You don't need this command when the provider is IDmelon.

```text
passkey-provisioner profiles (--add <name> | --list | --delete <name> | --edit <name> | --show <name>)
```

| Option | Description |
| --- | --- |
| `--add <name>` | Add a new profile interactively. |
| `--list` | List configured profiles. |
| `--delete <name>` | Delete an existing profile. |
| `--edit <name>` | Edit an existing profile interactively. |
| `--show <name>` | Show details for a profile. |

`--add` / `--edit` prompt for: UV required? (y/n) · UP required? (y/n) · Factory reset the security key before provisioning? (y/n) · Set a random PIN if needed? (y/n) · Random PIN length `[4]` · Wait to remove the provisioned security key? (y/n).

**Examples:**

```bash
passkey-provisioner profiles --add "Default"
passkey-provisioner profiles --list
passkey-provisioner profiles --show "Default"
passkey-provisioner profiles --edit "Default"
passkey-provisioner profiles --delete "Default"
```

### `providers`

Manage providers, activate one, and optionally assign a profile to it. Exactly one option at a time.

```text
passkey-provisioner providers (--add <name> | --activate <name> | --delete <name> | --edit <name> | --show <name> | --list)
```

| Option | Description |
| --- | --- |
| `--add <name>` | Add a new provider interactively. |
| `--activate <name>` | Set the active provider used for provisioning. |
| `--delete <name>` | Delete a provider. |
| `--edit <name>` | Edit a provider interactively. |
| `--show <name>` | Show provider details (secrets are masked). |
| `--list` | List configured providers. |

When adding/editing, you choose the provider type:

- **`[1] IDMELON`** — `api_key` and `endpoint` (default `https://skm.idmelon.com`).
- **`[2] MICROSOFT`** — `client_id`, `tenant_id`, `client_secret`, and optional custom Entra and Graph endpoints.

You can also assign a profile name to the provider; if the profile doesn't exist, profile creation is started.

**Examples:**

```bash
passkey-provisioner providers --add "Contoso"
passkey-provisioner providers --activate "Contoso"
passkey-provisioner providers --list
passkey-provisioner providers --show "Contoso"
passkey-provisioner providers --edit "Contoso"
passkey-provisioner providers --delete "Contoso"
```

### `device`

Manage the security key devices. Exactly one option at a time.

```text
passkey-provisioner device (--list | --select | --reset)
```

| Option | Description |
| --- | --- |
| `--list` | List all connected devices. |
| `--select` | List and select a device, and store it as the default in config. |
| `--reset` | Reset the default device in config. |

**Examples:**

```bash
passkey-provisioner device --list
passkey-provisioner device --select
passkey-provisioner device --reset
```

### `config`

Set or reset global configuration values. Use one option, not both.

```text
passkey-provisioner config (--reports-path <path> | --reset)
```

| Option | Description |
| --- | --- |
| `--reports-path "<path>"` | Set the folder where provisioning reports are written. |
| `--reset` | Clear the reports path (defaults are used). |

**Examples:**

```bash
passkey-provisioner config --reports-path "C:\IDmelon\Reports"
passkey-provisioner config --reset
```

### `create`

Provision passkeys for a single user (`--user`), for many users from a CSV (`--csv`) (When the provider is Microsoft), or through an SKM workflow (`--provisioning-id` / `--config`).

```text
passkey-provisioner create (--user <email> [--pin <PIN>] | --csv <path>) --profile <name>
passkey-provisioner create --provisioning-id <id> [--config <Base64Json>]
```

| Option | Description |
| --- | --- |
| `--user <email>` | Single target user. |
| `--pin <PIN>` | Optional security key PIN for single-user mode (use with `--user` only). |
| `--csv <path>` | CSV file of users for bulk creation. |
| `--profile <name>` | Provisioning profile to use. **Required** with `--user` or `--csv`. |
| `--provisioning-id <id>` | Workflow id for SKM pending-user polling (workflow mode). |
| `--config <Base64Json>` | Optional Base64-encoded JSON with a `baseUrl`/endpoint override. If unset, the active IDmelon provider endpoint is used. |

**Rules:**

- Provide exactly one input mode: `--user`, `--csv`, or `--provisioning-id`.
- `--profile` is **required** with `--user`/`--csv`, and **invalid** with `--provisioning-id`/`--config` workflow mode.
- Do not combine `--user` with `--csv`.
- `--pin` is valid only with `--user`; for `--csv`, use the optional `pin` column instead.
- Workflow mode (`--provisioning-id`/`--config`) rejects `--user`, `--csv`, `--pin`, and `--profile`.

**Examples:**

```bash
# Single user
passkey-provisioner create --user "alice@contoso.com" --profile "Default"

# Single user with an explicit PIN
passkey-provisioner create --user "alice@contoso.com" --pin "123456" --profile "Default"

# Bulk from CSV
passkey-provisioner create --csv "users.csv" --profile "Default"

# SKM workflow (typically launched automatically via the deep link)
passkey-provisioner create --provisioning-id "my-workflow" --config "eyJ..."
```

#### Users CSV format (`--csv`)

- The **first column** of each row is the user email; empty first-column values are skipped.
- If the first header cell is `email` or `user` (case-insensitive), that row is treated as a header and skipped.
- Optional column **`pin`** (case-insensitive): when present and non-empty for a row, it overrides the profile's default PIN for that user.

Email only (recommended):

```csv
email
alice@contoso.com
bob@contoso.com
```

With optional per-user PIN:

```csv
email,pin
alice@contoso.com,123456
bob@contoso.com,
```

No header (also valid):

```csv
alice@contoso.com
bob@contoso.com
```

## Use cases

### Admin-Managed

- **Bulk onboarding of a department or new hires.** Create one workflow for a group in the Admin Panel, then run the launched CLI to write passkeys to a stack of cards — one API key on the admin machine, many users provisioned in one sitting.
- **Central IT / help-desk issuance.** A dedicated station with the reader and the active IDmelon provider issues security keys for users who cannot self-provision.
- **Pre-provisioning of spare/replacement cards** ahead of distribution.

### Self-Service

- **Distributed or remote users.** Each user receives an emailed link and provisions their own card at their own desk — no admin machine or API key required.
- **Least-privilege provisioning.** No Admin API Key is stored on the endpoint; the user's own security key produces a short-lived access token, so nothing long-lived remains on the machine.
- **User-initiated key replacement.** A user who receives a new card can re-run their self-service link without waiting on IT.

## Reports and logs

- **Reports:** written to the folder set by `config --reports-path` (or the default location).
- **Logs:** `%programdata%\IDmelon\Passkey Provisioner\Reports\passkey-provisioner.log`.

## Troubleshooting

| Message / symptom | Cause and fix |
| --- | --- |
| `No arguments provided` | Pass a command and its required options. Run `passkey-provisioner --help`. |
| `Unknown command...` | Only `profiles`, `providers`, `device`, `config`, and `create` are supported. |
| `Either --user <email> or --csv <path> is required...` | Choose one input mode, or use workflow mode `--provisioning-id`. |
| `--profile <name> is required.` | Add `--profile` to `--user`/`--csv` create commands. |
| `Do not use --profile with workflow create...` | Workflow mode uses fixed workflow settings; remove `--profile`. |
| Reader not detected | Plug in a supported reader (for example, OMNIKEY 5022) and stop the **FCP**, and **Pairing Tool** services. |
| Deep link doesn't open the app | Reinstall the Passkey Provisioner so the `passkey-provisioner://` protocol is registered. |
