---
title: "IDmelon Reader Driver on Chromebook"
description: ""
lead: ""
date: 2023-09-11T12:58:04+03:30
lastmod: 2023-09-11T12:58:04+03:30
draft: false
images: []
menu:
  docs:
    parent: "idmelon_reader_driver"
weight: 321003
toc: true
---

IDmelon Accesskey, which is an extension, is designed and developed to enable you to leverage your card as a FIDO security key.

> Note: As a requirement, it needs the Smart Card Connector app to be installed.  

## Deployment

### Individual Chromebooks  

1. Install IDmelon Accesskey from [Chrome Store](https://chromewebstore.google.com/detail/idmelon-accesskey/naabfciimcjlmnohcligmgihhpihloah)  

2. Install Smart Card Connector from [Chrome Store](https://chrome.google.com/webstore/detail/smart-card-connector/khpfeaanjngmcnplbdlpegiifgpfgdco)  

3. If you are asked to give permission, click on the **Allow** button (it will happen only once):  

    ![alt](/images/vendor/idmelon_accesskey/Picture1.jpg)

### Managed Chromebooks (Business/School)  

1. Open Google [admin console](https://admin.google.com)  

2. Navigate to **Device -> Chrome -> Apps & Extensions -> Users & browsers**  

    ![alt](/images/vendor/idmelon_accesskey/Picture2.jpg)

3. Click on **+** button in the lower right corner and select **Add Chrome app or extension by ID**.  

    ![alt](/images/vendor/idmelon_accesskey/Picture3.jpg)

4. In the **Extension ID** enter **naabfciimcjlmnohcligmgihhpihloah** and click **Save**.  

    ![alt](/images/vendor/idmelon_accesskey/Picture4.jpg)

5. Once the extension is added, configure it to **Force install + pin to browser toolbar**  

6. Click on **Save** button on the top right corner.  

    ![alt](/images/vendor/idmelon_accesskey/Picture5.jpg)

7. Click on **+** button in the lower right corner and select **Add Chrome app or extension by ID**.  

8. In the **Extension ID** enter **khpfeaanjngmcnplbdlpegiifgpfgdco** and click **Save**.  

9. Once the app is created, configure it to **Force install**, and under **Policy for extensions**, write the following:  

    ```json
    {
      "force_allowed_client_app_ids": {
        "Value": [
          "naabfciimcjlmnohcligmgihhpihloah"
        ]
      }
    } 
    ```

10. If you are asked to give permission, click on **Allow** button (it will happen only once).  
