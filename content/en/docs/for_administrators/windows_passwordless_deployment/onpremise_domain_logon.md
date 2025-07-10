---
title: "On-premise Domain Logon"
description: ""
lead: ""
date: 2023-09-12T15:35:46+03:30
lastmod: 2025-01-20T15:35:46+03:30
draft: false
images: []
menu:
  docs:
    parent: "windows_passwordless_deployment"
weight: 88000
toc: true
---

While Microsoft solutions are available for Azure and Hybrid-Azure environments, they do not support local or on-premise Active Directory environments. In response, IDmelon has developed the IDmelon FIDO Credential Provider (IDmelon FCP). This application enables users with local or on-premise AD to use their security keys as well as other available environments, including IDmelon products, as a sign-in option. IDmelon FCP offers a secure and convenient way to log in to on-premises computers, with an easy installation and configuration process. To get started, refer to the "Getting Started" section below.  

## Getting Started  

1. Install IDmelon FCP, accessible on the IDmelon website's Downloads page.  
2. You are presented with two options for setup:  
    a. You can create a password from inside the IDmelon Panel. (Go to [Set Password from inside IDmelon Panel](/docs/for_administrators/windows_passwordless_deployment/nonentra_id_local_and_onpremise_domain_logon/#option-a-set-password-from-inside-idmelon-panel))  
    b. Alternatively, you can use FCP to log into Windows, so everything will be taken care of automatically. (Go to [Onboarding](/docs/for_administrators/windows_passwordless_deployment/nonentra_id_local_and_onpremise_domain_logon/#option-b-onboarding))  
3. Now, you or the user you took the steps above for can use a FIDO security key to log into Windows using IDmelon FCP.  

## Option a: Set Password from inside IDmelon Panel  

1. When logged into IDmelon Panel as an admin, choose a user from the **Users** section and go to the **Passwords** tab.  
2. Click the **Add Password** button and select either **Windows (Local account)** or **Domain** from the list.  
Note that by **Domain**, we mean that the user’s account is joined to an Active Directory.  
3. Follow the instructions to set the password.  

![alt](/images/vendor/DirectoryServicesIntegration/FCP/1.jpg)
![alt](/images/vendor/DirectoryServicesIntegration/FCP/2.jpg)
![alt](/images/vendor/DirectoryServicesIntegration/FCP/3.jpg)
![alt](/images/vendor/DirectoryServicesIntegration/FCP/4.jpg)

## Option b: Onboarding  

On the Windows lockscreen, when you are trying to log into Windows using a security key and the security key owner doesn’t have a password, with neither type of Domain or Windows local, you will see the onboarding page on the lockscreen as follows:  

![alt](/images/vendor/DirectoryServicesIntegration/FCP/5.jpg)

If the user's system is linked to either an Active Directory or an Azure Active Directory, and their User ID in the IDmelon Panel account follows an email format (username@domain.com), they will be prompted to enter a password for that account. Upon entering the password and if the domain to which the system is connected matches the valid domain of the user's User ID, they will be logged in. Subsequently, during next login attempts, they can log in using their security key.  

Alternatively, if the user's User ID in the IDmelon Panel account is not in email format, FCP identifies it as a local account and requests the user to input the password for that local account on the system. If the user is a valid username on that PC, the same process as above applies.  

If local users are currently logged into the system, a link appears to log into the last account they were logged into. Upon entering the password for that account, the same process as above ensues.  
Additionally, users have the option to sign in to a specific account by clicking the "Sign in with another account" link. This prompts them to enter a valid username and password for login into that account. Users must adhere to the following rules for username formats in this scenario:  

1. For a local account: Enter only the username without using '@' or '\\'. They can also include the PC name, such as 'PCNAME\username'.  
2. For an AD-joined account: Use the format username@domain.  
