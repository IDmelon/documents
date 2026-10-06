---
title: "Badge Readers and Cards"
description: "The badge readers and card types IDmelon Authenticator supports on shared Android devices, and how to set up badge login for them"
lead: "The badge readers and card types IDmelon Authenticator supports on shared Android devices, and how to set up badge login for them"
date: 2026-10-05T00:00:00+03:30
lastmod: 2026-10-05T00:00:00+03:30
draft: false
images: []
menu:
    docs:
        parent: "android_shared_device"
type: docs
weight: 321250
toc: true
---

Badge login reads an identifier from the card a user taps and matches it against the badges enrolled in your IDmelon
workspace. This page lists the readers and cards IDmelon Authenticator supports on a shared Android device, and the
`shared_login_method` settings that decide which reader is accepted and which identifier is read.

## Supported readers

| Reader                   | `model` value  | Reads the card serial | Reads HID PACS identifiers                                        |
|--------------------------|----------------|-----------------------|-------------------------------------------------------------------|
| USB smart card reader    | `smart_card`   | Yes                   | Yes, with a reader that supports HID's PACS commands             |
| Keyboard-wedge reader    | `keystroke`    | Yes                   | No                                                                |
| The phone's built-in NFC | `built_in_nfc` | Yes                   | No                                                                |
| IDmelon Hub              | `hub`          | Read by the Hub       | Read by the Hub                                                   |

By default every reader is accepted. Set `model` to a single reader to accept that one only — taps on any other reader
are then ignored without a message. The device reads badges only while IDmelon Authenticator is open on the screen.

### USB smart card readers

- Any USB smart card reader that follows the CCID standard and exchanges data at APDU level works. Readers that have
  been tested include the **ACS ACR1252** and the **HID OMNIKEY SE Plug**. There is no brand to configure: readers
  that report the serial in reverse byte order, such as RF IDeas readers, are recognized and corrected automatically.
- The device must support **USB host** (OTG). Connect the reader directly to a USB-C port or through an OTG adapter.
- The first time a reader is connected, Android asks whether IDmelon Authenticator may access it. Accept the prompt
  while you stage the device. Android asks again after the reader is reconnected or the device restarts.
- Reading **HID PACS** identifiers needs a reader that supports HID's PACS commands, such as an HID OMNIKEY reader.
  Other readers can still read the card serial. Readers that only release PACS data over HID's encrypted secure
  channel are not supported yet.

### Keyboard-wedge readers

These readers type the card's identifier as if it came from a keyboard, over USB or Bluetooth.

- Set the reader to type the card serial **as a decimal number followed by Enter**. Any other output — hex, or
  surrounding characters — is rejected with *"This device is not set up for this card."*
- Keys typed into a text field go to the field, and keys typed slower than a reader types are never mistaken for a
  badge.

### The phone's built-in NFC

The phone's NFC controller only ever sees the card serial, so it cannot read HID PACS identifiers. A phone held to the
device to present a mobile credential is refused, because it reports a different serial on every tap.

### IDmelon Hub

The Hub, previously referred to as the bridge, reads the card itself and sends the finished badge ID to the device.
The card settings described below do not apply to it.

## Supported cards

| Card                                    | Can be read as                                   | Set `config.type` to |
|-----------------------------------------|--------------------------------------------------|----------------------|
| MIFARE and other cards with a fixed serial | The card serial                               | `Default`            |
| HID iCLASS                              | The card serial, or HID PACS identifiers         | `Default`            |
| HID Seos                                | HID PACS identifiers only                        | `Seos`               |
| HID Prox                                | HID PACS identifiers only                        | `Prox`               |

- **HID Seos** cards answer only a reader that holds the keys for your Seos credentials. Without them the read fails
  and the user is asked to tap again.
- **HID Prox** is a low-frequency (125 kHz) card, so the reader must support Prox cards.
- A card whose serial changes on every tap cannot identify its holder, and is refused as unsupported. That covers
  every Seos card's serial, which is why Seos is always read through PACS, and phones presenting a mobile credential.

## Configure badge login

Badge settings go in the `shared_login_method` key of the
[app configuration policy](/docs/for_administrators/shared_devices/android_shared_device/config_with_intune/#step-3--create-the-app-configuration-policy):

```json
{
    "type": "badge",
    "model": "smart_card",
    "config": {
        "type": "Seos",
        "id_type": "PACSD",
        "prefix": "set"
    }
}
```

| Key              | Values                                                   | Default            | Meaning                                                                                       |
|------------------|----------------------------------------------------------|--------------------|-----------------------------------------------------------------------------------------------|
| `model`          | `auto`, `smart_card`, `keystroke`, `built_in_nfc`, `hub` | `auto`             | Which reader may deliver a badge ID. `auto` accepts all of them.                             |
| `config.type`    | `Default`, `Seos`, `Prox`                                | `Default`          | The card technology your site issues.                                                         |
| `config.id_type` | `CSN`, `PACSD`, `PACSH`, `PACSR`                         | Set by `config.type` | Which identifier to read: `CSN` for the card serial, or one of the HID PACS identifiers. Leave it out to read `CSN` from `Default` cards and `PACSD` from `Seos` and `Prox` cards. `Seos` and `Prox` cards have no usable serial, so they are always read as PACS, even with `CSN`. |
| `config.prefix`  | `set`, `reset`                                           | `set`              | `reset` drops the prefix from HID PACS identifiers.                                          |

Values are not case-sensitive. A value the app does not recognize falls back to the default, so a typo never locks
users out.

The minimal value, `{"type": "badge"}`, accepts every reader and reads the card serial.

> Choose these settings to match how badges are enrolled in your IDmelon workspace. The same card produces a
> different badge ID under each setting, and a tap read one way will not match a badge enrolled another way.

## Badge ID formats

The same card can be reported in several ways. The examples are from one HID Prox card, except the serial, which is
from a MIFARE card.

| Identifier    | Format                                                         | Example         |
|---------------|----------------------------------------------------------------|-----------------|
| `CSN`         | The card serial: uppercase hex, most significant byte first, no separators | `A3FCEF94` |
| `PACSD`       | `padd-`, then the facility code and card number in decimal     | `padd-139-1064` |
| `PACSH`       | `pahh-`, then the same two fields in fixed-width hex           | `pahh-8B-0428`  |
| `PACSR`       | `par-`, then the whole Wiegand bit stream in hex               | `par-1160850`   |

`PACSD` and `PACSH` split the credential into its fields, so they only work for the Wiegand formats the app
recognizes:

- **H10301**, 26-bit, with a facility code and card number.
- **H10302**, 37-bit, with a card number only. It has no facility code, so it is reported as `paxd-` or `paxh-`
  followed by the card number.
- **Corporate 1000**, 48-bit, with a company ID in place of the facility code.

For any other format, use `PACSR`, which reads the raw bit stream and works for every card. With `config.prefix` set to
`reset`, the same values are reported without their `padd-`, `pahh-`, `paxd-`, `paxh-`, or `par-` prefix. Use it only
if your badges are enrolled without those prefixes.

## What users see when a read fails

| Message on the device                                                         | What to check                                                                                                                                                       |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| The card could not be read. Please tap it again.                              | The card left the reader too soon, or the reader did not answer. A second tap usually works. A Seos card that keeps failing usually means the reader does not hold its keys. |
| This card is not supported. Please contact your administrator.               | The card cannot identify its holder with these settings — for example a Seos card read with `config.type` set to `Default`, or a phone presenting a mobile credential. |
| This device is not set up for this card. Please contact your administrator.  | The reader or the settings cannot produce the identifier asked for: a PACS identifier from the phone's NFC or a keyboard-wedge reader, a USB reader without HID's PACS commands, `PACSD` or `PACSH` for an unrecognized Wiegand format, or a wedge reader not typing a decimal number. |

## Troubleshooting

| Symptom                                               | What to check                                                                                                                                                       |
|-------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Taps on one reader are ignored without a message      | `model` accepts a different reader. Set it to `auto`, or to the reader in use.                                                                                       |
| A USB reader does nothing                             | Confirm the device supports USB host (OTG), and that the Android prompt to let IDmelon Authenticator access the reader was accepted after the last reconnect or restart. |
| A keyboard-wedge reader does nothing                  | The reader must end each read with Enter, and IDmelon Authenticator must be open on the screen with no text field selected.                                          |
| The badge is read, but the user is not recognized     | The identifier type or prefix does not match how the badge was enrolled, or the badge is not enrolled. Set `self_service_url` to send unenrolled badges to self-service. |
