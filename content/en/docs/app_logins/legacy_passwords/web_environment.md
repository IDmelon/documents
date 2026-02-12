---
title: "Web Environment"
description: "Log into all your web apps using your FIDO2 security key as a password vault."
lead: ""
date: 2024-09-25T09:00:00+03:30
lastmod: 2024-09-25T09:00:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "passwords_for_legacy_authentication"
weight: 67000
toc: true
---

In the web environment, users are able to use the **tap-to-login** solution for website accounts by the use of IDmelon card reader driver, IDmelon WebLogin Chrome extension and a Web Password which is assigned to the card owner in corresponding workspace in IDmelon panel.
While a webpage which contains credential forms is loaded and a registered card is tapped, if a web password for that URL exists for the card owner, it will be filled automatically.
In case the owner has no passwords for that URL, an onboarding prompt comes up to let the user add a web password for that URL immediately. This password will be saved for the card owner in IDmelon panel and can be used for later tap to login attempts.

### Step by step guide

1. Install IDmelon card reader driver.
2. Install IDmelon WebLogin extension from Chrome web store.
3. Navigate to the login page of the website for which you want to add a password and tap the card to start the onboarding procedure; Or you can add a new Web password through your workspace in IDmelon panel where you need to enter the URL of the login page. To make sure you have entered the right URL, click on the Extension icon in your browser while being in the login page and copy the URL which it recognizes.

> **Note:** Each password can require user verification to be filled. When the user verification is enabled for a password, the card PIN will be asked before filling the credentials.
>
> **Note:** To enable user verification for a password, either toggle the corresponding button in the IDmelon Admin Panel or enter the security key PIN in the PC onboarding prompt.
