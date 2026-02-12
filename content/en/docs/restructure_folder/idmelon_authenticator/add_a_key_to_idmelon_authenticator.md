---
title: "Add a Key to IDmelon Authenticator"
description: "How to add a new security key or passkey to the IDmelon Authenticator app."
lead: ""
date: 2022-02-16T18:14:02-08:00
lastmod: 2022-02-16T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "idmelon_authenticator"
weight: 303000
toc: true
---

You can add the following platforms to your security key.  

## Windows Login  

- This feature is only available for Azure Active Directory joined PCs. Standalone PCs or even on-premise AD do not support this feature.  
- To enable this feature on your Azure AD joined PCs, call your IT administrator.  

Note: Administrators for a technical guide can refer to this [link](/docs/for_administrators/windows_passwordless_deployment/entra_id_connected_pcs).  

## Office 365  

1. Sign in to your Microsoft account at `https://myaccount.microsoft.com/` and go to the My Profile page.  
2. Choose Security Info and then Add sign-in method to see a list of different methods. Next, choose Security key from the list.  
3. Choose USB device.  
4. In the Setting up your new sign-in method page, select Next.  
5. If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
6. If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
7. You will return to the Security info page, type a name you'll recognize later for your new security key, and then select Next.  
8. Select Done to close the Security key page. The Security info page is updated with your security key information.  

## GitHub  

1. Before adding a security key to your GitHub account, you must first enable 2FA via a TOTP mobile app or via SMS. [Check here](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication).  
2. Sign in to your GitHub account.  
3. In the top-right corner of any page, click your profile photo, then choose Settings.  
4. In the Access section of the sidebar, click Password and authentication.  
5. Next to Security keys, click Add.  
6. Under Security keys, click Register new security key.  
7. Type a nickname for the security key, then click Add.  
8. If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
9. If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
10. The Security keys section is updated with your security key information.  

## Twitter  

1. Log in to your Twitter account. In the side menu on the left, click More, then click Settings and privacy.  
2. Click on Security and account access, and then click Security.  
3. In the Two-factor authentication section, choose Security key from the list of methods.  
4. You will be asked to enter your password to continue.  
5. After you click Continue:  
6. If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
7. If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
8. Click Add key at Add the security key to your Twitter account page.  
9. Enter a name for your security key at the Name your security key and click Next.  
10. Click Done.  

## Facebook  

- To add a security key on your desktop, you’ll need to be using the latest version of Chrome, Opera, Firefox, Edge, or Safari.  
- To add a security key:  

1. Go to your Security and login settings.  
2. Scroll down to Use two-factor authentication and click Edit. You may be asked to re-enter your Facebook password at this point.  
3. Click Use security key.  
4. Click Register security key then follow the on-screen instructions. (If you do not see the Register Security Key option, you may need to update your browser.)  
   - If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
   - If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
5. Once you’ve successfully added a security key to your account, you can start using it when you log in.  

## AWS  

- You can enable a FIDO security key for your own IAM user from the AWS Management Console only, not from the AWS CLI or AWS API.

1. Use your AWS account ID or account alias, your IAM user name, and your password to sign in to the IAM console. To get your AWS account ID, contact your administrator.  
2. In the navigation bar on the upper right, choose your user name, and then choose Security Credentials.  
3. On the AWS IAM credentials tab, in the Multi-factor authentication (MFA) section, choose Activate MFA.  
4. In the Manage MFA device wizard, choose Security key, and then choose Continue.  
5. Then,  
   - If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
   - If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
6. Say yes to the notification and choose Close when setup is complete.  
   - The IDmelon FIDO2 security key is ready for use with AWS.  

## Bitbucket  

- Bitbucket supports security keys that use the FIDO U2F standard. These keys only work with a recent version of Google Chrome, which means that you need to install or update your browser if you want to begin using a security key.  
- Before adding a security key to your Bitbucket account, you must first enable Two-step verification.  
- To add a security key:  

1. From your avatar in the bottom left, click Personal settings.  
2. Click Two-step verification under Security.  
3. Under Security keys, enter a name for your device in the text box.  
4. Click Add security key:  
   - If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
   - If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  

## Cloudflare  

- To enable 2FA security key authentication after enabling 2FA on your Cloudflare account by clicking the Manage button:  

1. In Security Key Authentication, click Add.  
2. Enter your Cloudflare password on the Add a Security Key screen, then click Next.  
3. A dialog appears. Interact with your security key to add it to your Cloudflare account.  
   - If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
   - If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
4. Enter a name for the security key. If this is the initial setup, you will be prompted to generate backup codes. If not, skip to Step 8.  
5. Enter your password.  
6. Click Next again to review your backup codes. Backup codes can be used to access your user account without your mobile device.  
7. Click Download, Print, or Copy to save your backup codes in a secure location.  
8. Click Next to finish the configuration.  

## Coinbase  

- To set up your security key:  

1. Sign into your Coinbase account from a web browser. (You cannot change your 2-step method from the Coinbase mobile app.)  
2. Go to Settings > Security.  
3. Select Security Key under 2-step verification.  

- For more information check [this article](https://help.coinbase.com/en/coinbase/managing-my-account/verify-my-identity/using-and-managing-security-keys).  

## Dropbox  

1. Sign in to dropbox.com.  
2. Click your avatar.  
3. Choose Settings.  
4. Select the Security tab.  
5. Under Two-step verification, click Add next to Security keys. If you do not see this section, follow the Enable two-step verification instructions before proceeding.  
6. Enter your password.  
7. Then click Begin setup.  
8. Click Key inserted.  

- If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.
- If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.

> **Note**: Dropbox only supports using a security key when signing in to dropbox.com using the Chrome or Firefox web browsers. You can’t use a security key to sign in to the desktop or mobile apps at this time.  

## ebay  

1. Sign in to your ebay account.  
2. Go to Account Setting.  
3. Choose Sign in and Security under the Personal Info.  
4. Scroll down to see Security key sign in.  
5. Click Turn on.  
6. Enter your Password.  
7. Click Continue on the new page that will be shown as Setup your security key.  

- If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
- If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  

## Salesforce  

- If your Salesforce admin has allowed the use of Universal Second Factor (U2F) security keys, register your own security key to connect it to your account.  

1. From your personal settings, enter Advanced User Details in the Quick Find box, then select Advanced User Details. No results? Enter Personal Information in the Quick Find box, then select Personal Information.  
2. Click Register next to the Security Key (U2F) field.  
3. If you don’t see this option, your Salesforce admin has disallowed the use of security keys.  
4. For security purposes, you’re prompted to log in to your account.  
5. At the prompt:  
   - If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
   - If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
6. After successful registration, click Continue to dismiss the confirmation message.  

## Shopify  

- From your Shopify admin, click your username and account picture.  

1. Click Manage account > Security.  
2. In the Two-step authentication section, click Turn on two-step.  
3. Select Security key or Biometrics, then click Next.  
4. Enter a name for your security key.  
5. Enter your password.  
6. Click Add device.  
7. Activate your security key:  

- If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
- If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
- You are provided with a list of 10 recovery codes. Save your recovery codes in case you lose access to your mobile device. Make sure that you store them in a safe location offline that you can access in multiple ways, such as from your mobile device, your desktop computer, and from a printed document.  

## Yahoo  

1. Sign in to the Yahoo Account security page.  
2. Next to 2-Step Verification, click Turn on 2SV.  
3. Click Get started.  
4. Select Security key.  
5. Click Set up Key.  
6. Follow the on-screen prompts to finish setting up your Security Key.  

- If you are using an IDmelon Reader, insert it into the USB port of your device, tap your smartphone to the Reader which starts blinking blue for a few seconds to receive the notification.  
- If you are using IDmelon Pairing Tool, you will receive the notification on the smartphone which is already paired with your computer.  
