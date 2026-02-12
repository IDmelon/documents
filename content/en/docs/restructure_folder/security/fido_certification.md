---
title: "FIDO Certification"
description: "IDmelon FIDO certification and compliance for security keys."
lead: ""
date: 2022-02-16T18:14:02-08:00
lastmod: 2022-02-16T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "security"
weight: 910000
toc: true
---

## What is FIDO?

---

FIDO is an open authentication protocol to eliminate passwords and their related security threats. To use FIDO, user first must complete the registration process and selecting the method of authentication (biometric factor). Then users can log into FIDO supported products or services by only providing a biometric factor which can be a fingerprint, face ID (look in the camera), or entering a PIN. It depends on the available capabilities of users’ smartphone and methods the product or service can handle. The process of authentication from user’s point of view is simple and only needs providing the biometric factor, the rest of the process is done behind the scenes without even noticed. For more information about FIDO, you can visit FIDO alliance [here](https://fidoalliance.org).

## FIDO security

---

Passwords are hard to remember, need to be changed frequently and also need resources when reset required. The FIDO2 helps stop the password chaos and improve the user experience. It uses standard public key cryptography to secure user authentication by encrypting all  communications. Private keys and biometric information if used to authenticate never leave users’ devices, which lessens the chances of discovering them during transmission and makes authentication processes stronger and more secure.

FIDO2 is built with FIDO Client To Authenticator Protocol (CTAP) and W3C standard WebAuthn which both work together to provide passwordless authentication experiences for users. These experiences might involve embedded authenticators, like biometrics or PINs or USB devices. WebAuthn defines a standard web API for platforms and browsers to support FIDO which communicates with CTAP authenticators to provide an interface for creating and managing public key credentials. CTAP allows the authenticator (part of the user’s computer or smartphone, or an external piece of hardware) to be used as both the first and second factor of authentication to provide users a passwordless authentication experience.

When a user attempts to access an online service for the first time, they are prompted to register. During registration, a new key pair is generated that the private key is stored on the device and associated with the id and domain of the online service, while the public key is stored in the online service’s key database on a server. Each time the user attempts to subsequently access the service, the online service uses APIs to verify user credentials with the authenticator. When the user attempts to sign on to an application, a FIDO2 server, sends the FIDO client (browser, desktop or mobile application) a challenge using WebAuthn, asking it to sign the data with the private key. The user consents to the request by using a method of authentication. The domain is checked against the domain that was associated at registration time which gives FIDO its **strong phishing resistance**. The client obtains the private key from the authenticator and answers the request in a way that proves the device has possession of the private key to gain access to the online service for user. From now on communication is
encrypted throughout this process, and private keys never leave users’ devices.

## FIDO Certification

---

IDmelon is part of FIDO Alliance whose mission is to develop and promote authentication standards at the client and protocol layers that “help reduce the world’s over-reliance on passwords.” Long before IDmelon becomes an official member of the Alliance, IDmelon products were FIDO certified. And now following its entry into the Alliance, which is rapidly growing and is supported by Microsoft, Google, Apple, and all the leading security service providers in the industry, IDmelon will remain committed to collaborating with FIDO Alliance and cybersecurity industry leaders to offer the world advanced security solutions that are tailored exactly to their needs.

To visit our showcase that is live on the FIDO Alliance website, please
click [here](https://fidoalliance.org/company/idmelon).
