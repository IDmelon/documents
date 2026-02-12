---
title: "PingOne DaVinci"
description: "How to integrate IDmelon with PingOne DaVinci for single sign-on."
lead: ""
date: 2023-12-15T23:05:00+03:30
lastmod: 2023-12-15T23:05:00+03:30
draft: false
images: []
menu:
  docs:
    parent: "app_integration_guides"
weight: 72610
toc: true
---

You can use the IDmelon connector as part of a PingOne DaVinci flow in order to improve user experience by allowing users to go passwordless and authenticate with their smartphones, ID badges, or access cards as a FIDO security key.

In this guide, we'll walk you through the process of integrating and using the IDmelon Connector in Davinci, with a simple example to display user information after successful login.

## PingOne DaVinci Configuration

1. In the PingOne dashboard, click on the `Flows` menu, and then press the `Add Flow` button.

    ![alt](/images/vendor/davinci/01.png)

2. Create a Blank Flow.

    ![alt](/images/vendor/davinci/02.png)

3. Fill the value of Flow form for `Name` and `Description` fields and then click on the `Create` button.

    ![alt](/images/vendor/davinci/03.png)

    ![alt](/images/vendor/davinci/04.png)

4. In the Blank created flow page, click on the `+` button.

    ![alt](/images/vendor/davinci/05.png)

5. From the pop up menu, click on the `New` tab.

    ![alt](/images/vendor/davinci/06.png)

6. Search for `IDmelon` connector by typing and select the `IDmelon` from the displayed item.

    ![alt](/images/vendor/davinci/07.png)

7. In the opened dialog form, click on the `Create` button.

    ![alt](/images/vendor/davinci/08.png)

8. You now see  the `IDmelon` Connector successfully created and added to the flow page.

    ![alt](/images/vendor/davinci/09.png)

9. Click on the `IDmelon` connector in the flow page and then click the `Configure` button from the opened right panel.

    ![alt](/images/vendor/davinci/10.png)

10. Fill the fields as describe:

    Provider name: Enter a custom name for it.

    Auth Type: Chose the `Oauth2`.

    Redirect URL: Copy this value, we will use this value in the IDmelon Panel, mentioned in step `24`

    Issuer URL: Empty this value as is.

    Authorization Endpoint: <https://sso.idmelon.com/api/oidc/idp/authorize/>

    Token Endpoint: <https://sso.idmelon.com/api/oidc/idp/token/>

    User Info Endpoint: <https://sso.idmelon.com/api/oidc/idp/userinfo/>

    Client ID: Get this value from the IDmelon Panel, mentioned in step `24`

    Client Secret: Get this value from the IDmelon Panel, mentioned in step `24`

    Scope: `openid email profile`

    Note: Minimum valid value for `Scope` is `openid`

    ![alt](/images/vendor/davinci/11.png)

    ![alt](/images/vendor/davinci/12.png)

11. After completing the form fields as described, click on the `Apply` button.

    ![alt](/images/vendor/davinci/13.png)

12. In the flow page, click on `+` icon and add `HTTP` connector.

    ![alt](/images/vendor/davinci/14.png)

    ![alt](/images/vendor/davinci/15.png)

13. Now in the flow page, Connect `IDmelon` Connector  to the `HTTP`Connector.

    ![alt](/images/vendor/davinci/16.png)

    ![alt](/images/vendor/davinci/17.png)

14. Click on the `HTTP` connector and from the right side opened panel, select the `Custom Html Message`

    ![alt](/images/vendor/davinci/18.png)

15. From the `General` tab, from the `Message` field, click on the `{}` icon.

    ![alt](/images/vendor/davinci/19.png)

16. Chose the `IDmelon` from the opened list.

    ![alt](/images/vendor/davinci/20.png)

17. Select the output data with clicking on the `+`  button.

    ![alt](/images/vendor/davinci/21.png)

    ![alt](/images/vendor/davinci/22.png)

18. From the flow panel, Save your flows by clicking on the `Save` button from the top right buttons.

19. Deploy your flow, with Clicking the `Deploy` button.

20. To test the flow, click the `Try Flow` button.

    ![alt](/images/vendor/davinci/23.png)

21. Now based on the created flow, you will redirect to `IDmelon` to login with your passkey, your security key or other options.

    ![alt](/images/vendor/davinci/24.png)

22. After successfully logging into IDmelon, the requested data is provided to the next flow mentioned in the flow page as `HTTP` and will be displayed.

    ![alt](/images/vendor/davinci/25.png)

## IDmelon Panel configuration

1. You need to create an OIDC Application, to do it, go to this address:

    <https://sso.idmelon.com/api/oidc/idp/applications/>

    And then click on the `Click here`.

    ![alt](/images/vendor/davinci/26.png)

2. For new displayed application form, consider as below:

    Name: Enter your desired name.

    Client id: You need to copy this value, and use this value in the step `10` as `Client ID`

    Client secret: You need to copy this value, and use this value in the step `10` as `Client Secret` Remember this value show only once, and then the value will be show as encrypted, and is not usable, so make sure that you copy it.

    Authorization grant type: Authorization code

    Redirect uris: You need to paste the value of `Redirect URL` field that you copied in step `10` into this field.

    Algorithm: HMAC with SHA-2 256

    And finally click on the `Save` button.

    ![alt](/images/vendor/davinci/27.png)

## Tips

OpenID Connect Well-Known Configuration:

<https://sso.idmelon.com/api/oidc/idp/.well-known/openid-configuration/>
