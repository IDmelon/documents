---
title: "Troubleshooting Instructions"
description: "Kiosk App Troubleshooting Instructions"
lead: ""
date: 2025-04-11T18:14:02-08:00
lastmod: 2025-04-11T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "idmelon_kiosk"
weight: 30
toc: true
---

## Collect App Logs

If the Kiosk app is not working correctly, share the files below with IDmelon support:

```shell
C:\Users\kioskUser0\AppData\Local\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\app_logs.log
C:\Users\kioskUser0\AppData\Local\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\extension_version.xml
C:\Users\kioskUser0\AppData\Local\Packages\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\configs.xml
C:\ProgramData\IDmelon\Accesskey\service_logs.log
```

## FAQ

{{< faq question=`What is the minimum required OS for the Kiosk app?`
answer=`Windows 10 and 11 x64 (Build number 1809) and later.` >}}

{{< faq question=`What is the minimum required version of Microsoft Edge?`
answer=`Microsoft Edge WebViewCore 131.0.0.0 and later. (Located on C:\Program Files (x86)\Microsoft\EdgeWebView)` >}}

{{< faq question=`Nothing happens when I tap the card on the reader.`
answer=`

- Make sure the Accesskey service is running with the following command:<br>
**accesskeycli status**
- Make sure the workflow is configured correctly with the following command:<br>
**accesskeycli workflow-automation**
- Make sure the card is assigned to the user with the following command:<br>
**accesskeycli health-check**
- Make sure the Web Login Extension is enabled by checking the app configuration at the following path:<br>
**C:\Users\kioskUser0\AppData\Local\Packages**
**\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\configs.xml**
- Ensure the extension was downloaded and installed correctly by checking for the file<br>
**C:\Users\kioskUser0\AppData\Local\Packages**
**\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\extension_version.xml**.
- If you have set any URL other than **https://myapps.microsoft.com** as **Kiosk URL**, make sure the **SelfService** option is set as **true** in the kiosk configuration in the following path:<br>
**C:\Users\kioskUser0\AppData\Local\Packages**
**\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\configs.xml**
`>}}

{{< faq question=`After the automation steps, I encountered a Microsoft login error.`
answer=`Make sure the server addresses are set correctly if you are using a dedicated system (on-premises):<br>
**accesskeycli server-address**<br>
By checking the server address set in the kiosk app configuration:
**C:\Users\kioskUser0\AppData\Local\Packages**
**\Hellokey.45853B8ADE74A_kxcedb3gts26c\LocalState\configs.xml**` >}}

{{< faq question=`Is it possible to enroll unregistered cards in the Kiosk app?`
answer=`To enable this feature:

- Log in to the **IDmelon admin panel** and get the **self-service** URL from the following path:<br>
**Security Keys > Workflows > Self-Service Actions**
- Enter the following command in a PowerShell:<br>
**accesskeycli self-service-link -s [Self-Service URL]**
` >}}
