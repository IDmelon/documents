---
title: "Provision FIDO2 Passkeys for Okta"
description: ""
lead: ""
date: 2025-07-31T12:00:00+03:30
lastmod: 2025-07-31T12:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "manage_passkeys_and_credentials"
weight: 34110
toc: true
---

Admins can assign Okta passkeys to users via the IDmelon Admin Panel by integrating their Okta environment. Once configured, users can use their security keys to authenticate to Okta-supported applications and services.

## Prerequisites

- Ensure you have an **Okta origin** with access to the **Okta Admin Console**.
- Okta users must be available and synchronized in the IDmelon Admin Panel (via group or directory import).
- You must have an **Okta group** containing users to which the WebAuthn policy will be applied.

### Generating an Okta API Token

- Navigate to **Security > API** then select **Tokens** and click on **Create Token**.

  ![okta1](/images/vendor/provisioning/okta/01.png)

- Select a Name for the token
- Select `Any IP` or any restriction see fits
- Click **Create Token**

  ![okta2](/images/vendor/provisioning/okta/02.png)

> Generated API token will be needed in IDmelon app integration.
> The API token will inherit all the privileges of the user who created it.

### Setting up Okta FIDO2 Authenticator options

- Navigate to **Security > Authenticators** then select **Setup** options and click **Add authenticator**.

  ![okta3](/images/vendor/provisioning/okta/03.png)

- Select FIDO2 (WebAuthn) authenticator.

  ![okta4](/images/vendor/provisioning/okta/04.png)

- Select **Preferred** option and then click **add**
- Click **Add**
- Fill required fields as described in picture:
  - Policy name

  ![okta5](/images/vendor/provisioning/okta/05.png)

### Add Okta Enrollment policy

- Navigate back to **Security > Authenticators** then select **Enrollment** options
- Click **Add a policy**

  ![okta6](/images/vendor/provisioning/okta/06.png)

- Fill required fields as described in picture:
  - Policy name
  - Assign to groups -> Groups that wish to use FIDO2 (webAuthn) authenticator
  - Authenticators -> Select authenticator options as fit
  - Allowed authenticators -> Any webAuthn authenticator

  ![okta7](/images/vendor/provisioning/okta/07.png)

- In newly opened window `Add Rule` provide a rule name
- Click **Create Rule**

  ![okta8](/images/vendor/provisioning/okta/08.png)

### Setup Okta Authentication policy

- Navigate back to **Security > Authentication Policies** then select **Okta Dashboard** _(Or any application)_

  ![okta9](/images/vendor/provisioning/okta/09.png)

- In **Rule** section, Click **Add Rule**

  ![okta10](/images/vendor/provisioning/okta/10.png)

- Fill required fields as described in picture:
  - provide **Rule name**
  - Assign this rule to same groups which authenticator policy is enabled
  - Select Possession factor
  - Select Phishing resistant option
  - Select FIDO2 (WebAuthn) authenticator

  ![okta11](/images/vendor/provisioning/okta/11.png)

  ![okta12](/images/vendor/provisioning/okta/12.png)

### Integrate IDmelon with Okta

- Login to IDmelon Admin Panel then navigate to **App Integration > API**
- Select **Okta API** option
- Fill required fields as described in picture:
  - Origin -> Okta workspace domain
  - Okta API Key

![panel1](/images/vendor/provisioning/okta/p01.png)

---

Now Specified group users can only access specified application when all authentication policies met.

## Assign Okta Passkey One by One

Admins can assign Okta passkeys individually through the user profile in the Admin Panel.

1. Log in to the **IDmelon Admin Panel**.
2. Navigate to **Users → All Users**.
3. Select the user you wish to assign a passkey to.
4. Go to the **Passkeys** tab.
5. Click **Add Passkey** and choose **Okta**.
6. Click **Create** in the popup window.

The passkey will be assigned to the user and will become active once the security key is initialized by the user.

## Automatic Okta Assignment via Workflows

Admins can automate the assignment of Okta passkeys using the workflow feature.

1. Navigate to **Workflows → Okta Passkey Provisioning**.
2. Enter a name for the workflow (e.g., "Auto Okta Provisioning") and click **Next**.
3. On the configuration page:
   - The **Okta Origin** field will be automatically populated if the Okta integration has already been set up.
4. Set the **trigger** condition:
   - **New User Imported:** Assigns the passkey when a user is added to the panel.
   - **New Security Key Activated (Recommended):** Assigns the passkey once the user activates their security key.
5. Continue to the next step to **review** and **submit** the workflow.

Once activated, the workflow will automatically assign Okta passkeys to applicable users based on the chosen trigger, requiring no further manual action.
