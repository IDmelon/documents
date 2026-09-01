---
title: "Passkey Provisioner Tool"
description: ""
lead: ""
date: 2025-04-15T15:00:00+03:30
lastmod: 2026-09-01T15:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "manage_passkeys_and_credentials"
weight: 34090
toc: true
---

The **IDmelon Passkey Provisioner** is a Windows desktop application that writes FIDO2 passkeys onto physical security keys for your users. It works together with a **provisioning workflow** created in the IDmelon Admin Panel and can operate in two modes:

- **Admin-Managed** — an administrator provisions passkeys on behalf of users.
- **Self-Service** — users provision their own passkey.

Provisioning always starts from the Admin Panel: when you (or a user) click the provisioning link, the browser opens the application through the `passkey-provisioner://` deep link and the provisioning run starts automatically. The application guides the operator with step-by-step animations and messages that match the selected hardware type (for example, holding a Crescendo Card on a reader, or inserting and touching a USB security key).

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
   - **Admin-Managed:** select **Open passkey-provisioner** when the browser alert appears. The application window opens, polls the workflow, and provisions each pending user with the connected security key.
   - **Self-Service:** each selected user receives an email with a provisioning link. The user opens the link, follows the on-screen steps, and selects **Open passkey-provisioner** when the alert appears.

> **Note:** When a run completes, each user's **status** is updated in the Admin Panel. Close the current workflow and start a new one for the next batch.

## Admin-Managed mode

In Admin-Managed mode you register an Admin API Key on the machine once, then provision users from the workflow.

### 1. Generate an Admin API Key

In the [IDmelon Admin Panel](https://panel.idmelon.com), go to **Authentication → API Key Management → New API Key** and create a key with **Type: Admin**. Copy the generated key.

![API Key Management - New Admin API Key](/images/vendor/passkey_provisioner/panel_api_key.png)

### 2. Register an IDmelon provider

Register the API key locally as a provider in the application:

1. Open **IDmelon Passkey Provisioner** from the Start Menu.
2. In the **Providers** section, click **+ Add** and fill in:
   - **Name** — a label for the provider (for example, your tenant name).
   - **API Key** — paste the Admin API Key from step 1.
   - **IDmelon endpoint** — the base URL (default `https://skm.idmelon.com`).
3. Click **Add**. The first provider you add automatically becomes the **active** provider (marked with an **ACTIVE** badge).

If several providers are registered, select one and click **Activate** to choose which one is used for provisioning. **Show** displays the provider details with the API key masked, **Edit** updates the endpoint or replaces the API key (leave the API key field empty to keep the current one), and **Remove** deletes the provider.

### 3. Create and run the workflow

Follow [Creating a provisioning workflow](#creating-a-provisioning-workflow-in-the-admin-panel) above, selecting **Admin Managed** in step 6. When you click **Open passkey-provisioner**, the application opens and provisions each pending user, guiding you through every step — insert or hold the security key, touch it when asked, and remove it when the run tells you to.

## Self-Service mode

Self-Service mode needs **no API key and no provider** — steps 1 and 2 of Admin-Managed do **not** apply. The user authenticates with their own security key to obtain an access token, then provisions their own passkey.

1. An administrator creates the workflow (see [Creating a provisioning workflow](#creating-a-provisioning-workflow-in-the-admin-panel)), selecting **Self-Service** in step 6.
2. The user opens the provisioning link sent to their email address.
3. The user follows the on-screen steps and selects **Open passkey-provisioner** when the alert appears.
4. When provisioning starts, the application first prompts the user to **touch their security key** to obtain an access token, then provisions the passkey. The user's name appears in the window as soon as it is resolved.

## The application window

Opening **IDmelon Passkey Provisioner** from the Start Menu (without a provisioning link) shows the management view.

![App GUI](/images/vendor/passkey_provisioner/gui_main.png)

### Providers

Registers the Admin API Keys used for Admin-Managed provisioning. Select a provider in the list to use the actions below it:

| Action | Description |
| --- | --- |
| **+ Add** | Add a new provider (name, API key, endpoint). The first provider becomes active automatically. |
| **Activate** | Make the selected provider the one used for provisioning. |
| **Show** | Show provider details; the API key is masked. |
| **Edit** | Change the endpoint or replace the API key (an empty API key field keeps the current one). |
| **Remove** | Delete the selected provider. |

### Security keys

Lists the security key devices currently connected to the machine. Click **Refresh** to scan again — useful for checking that a reader or key is detected before starting a run.

### Settings

The **gear icon** in the title bar opens Settings, where you set the **reports & logs path** — the folder where the application writes its log file. Leave it empty to use the default location, `%programdata%\IDmelon\Passkey Provisioner\Reports`.

### Logs

Two title-bar icons work with the application log:

- **Open log file** — opens `provisioner_logs.log` from the reports & logs folder.
- **Export logs** — packs the log file and its rotated backups into a **zip archive** through a save dialog, ready to share with support.

## During a provisioning run

When a run starts (from the browser deep link), the window switches to the provisioning view:

- **User list (left):** every user in the run with a live status — pending, provisioning (highlighted), completed (green check), or failed (red cross; hover for the reason). A counter shows the overall progress (for example, **2 of 5**).
- **User PIN:** in Admin-Managed runs, when the workflow assigns a security key PIN to a user, a masked **PIN ••••** line appears under the user's name. Click the **eye icon** to reveal or hide it — for example, to hand the PIN to the user after their key is provisioned. PINs are hidden by default.
- **Animation and step message (center):** the current step is shown with an animation matching the hardware type and a message such as *Hold your card on the reader*, *Insert your security key*, *Touch your security key*, *Resetting your security key*, *Registering passkey* (with the relying party shown underneath), *Remove the card*, or *Remove the security key*.
- **PIN prompt:** if the security key requires PIN entry during the run, the application asks for it in a dialog.
- **Cancel:** aborts the run after a confirmation. The operation in progress is stopped immediately — like unplugging in the middle of an operation — so only cancel when necessary.
- **Result:** when the run finishes, the window shows the outcome (completed, completed with errors, or failed). Per-user results are recorded in the Admin Panel.

## Use cases

### Admin-Managed

- **Bulk onboarding of a department or new hires.** Create one workflow for a group in the Admin Panel, then let the application provision a stack of cards one after another — one API key on the admin machine, many users provisioned in one sitting.
- **Central IT / help-desk issuance.** A dedicated station with the reader and a registered IDmelon provider issues security keys for users who cannot self-provision.
- **Pre-provisioning of spare/replacement cards** ahead of distribution.

### Self-Service

- **Distributed or remote users.** Each user receives an emailed link and provisions their own card at their own desk — no admin machine or API key required.
- **Least-privilege provisioning.** No Admin API Key is stored on the endpoint; the user's own security key produces a short-lived access token, so nothing long-lived remains on the machine.
- **User-initiated key replacement.** A user who receives a new card can re-run their self-service link without waiting on IT.

## Troubleshooting

| Symptom | Cause and fix |
| --- | --- |
| Deep link doesn't open the app | Reinstall the Passkey Provisioner so the `passkey-provisioner://` protocol is registered. |
| Reader not detected | Plug in a supported reader (for example, OMNIKEY 5022), click **Refresh** under **Security keys**, and stop the **FCP** and **Pairing Tool** services. |
| "No IDmelon provider is configured" | Admin-Managed runs need a provider: open the application and add one under **Providers**. |
| "The selected IDmelon provider has no api_key" | Edit the provider under **Providers** and enter a valid Admin API Key. |
| Provisioning failed for a user | Hover the red status in the user list for the reason, check the Admin Panel statuses, and use **Export logs** to collect the log file for support. |
| The security key rejects the PIN | The application re-prompts for the PIN; verify it with the user, or reset the key through a workflow with the Reset option checked. |
