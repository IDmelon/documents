---
title: "What is a Reader?"
description: "Overview of the IDmelon Reader hardware and how it works to use smartphones as security keys."
lead: ""
date: 2022-02-16T18:14:02-08:00
lastmod: 2022-02-16T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "idmelon_reader"
weight: 341000
toc: true
---
IDmelon Reader is a hardware technology designed and developed by IDmelon Technologies to let users use their smartphones as a security key.
As an example, users on a shared or personal computer can enjoy the passwordless login experience with a single tap with a smartphone on the reader.
IDmelon Reader supports different modes of operation each of which has different capabilities and features. In this document and the following pages,
we will describe how the reader works in different modes and how you can switch between these modes.

## Hardware

---

![alt](/images/vendor/ReaderGuide/reader_guide_1.png)

## Basic Concepts

---

IDmelon Reader supports 3 different working modes.
Each mode has different capabilities and designed for a use case. Users can switch and choose their working mode according to their needs and use cases.

### Smartphone BLE Reader

In this mode, the hardware works as a BLE reader and can distinguish the TAP of the user’s smartphone, similar to the experience of TAP
with access cards on NFC or RFID readers or Tap to Pay for iPhone and Android phone.
There is no need to pair the smartphone with the reader and only having the IDmelon Authenticator installed on the smartphone and keeping
the smartphone’s Bluetooth on is enough to work.

### FIDO2 USB Security Key

In this mode, IDmelon hardware works as a FIDO2 USB security Key by itself. The function is the same as other FIDO USB security keys,
and the user authentication for token is done by user PIN.
There is no need to have a smartphone in this mode, the hardware works independently as a FIDO security key.

### Paired Smartphone

To have the smartphone as a FIDO2 security key in this working mode, the users must pair their smartphone with the reader.
This mode is not suitable for shared computers, but has advantages over the first mode for users on personal computers.
