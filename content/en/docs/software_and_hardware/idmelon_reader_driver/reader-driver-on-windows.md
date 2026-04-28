---
title: "IDmelon Reader Driver on Windows"
description: ""
lead: ""
date: 2023-09-11T12:58:04+03:30
lastmod: 2023-09-11T12:58:04+03:30
draft: false
images: []
menu:
  docs:
    parent: "idmelon_reader_driver"
weight: 321002
toc: true
---

The IDmelon Reader Driver, which is a combination of a Windows service and a driver, is designed and developed to enable users to leverage their card, fingerprint, or face as a FIDO security key.

## Configuration

The IDmelon Reader Driver on Windows is configurable in many aspects. The most important configuration options are described below.

### Server Configuration

Server configuration is used in dedicated deployment scenarios, such as on-premises deployments, and for authenticated client configuration in Reader Driver version 3.13.0 and later.

#### On-Premises Server Configuration

To connect the Reader Driver to an on-premises server, set the API URL of that server in the Reader Driver using the following command:

```shell
accesskey config server set --api-url https://skm.sample.com/api/url
```

#### API Key Configuration (v3.13.0+)

To authenticate the Reader Driver, configure the API key using the following command:

```shell
accesskey config server set --api-key API_KEY_VALUE
```

#### Workspace ID Configuration (v3.13.0+)

To configure the Reader Driver to target a specific workspace, set the workspace ID using the following command:

```shell
accesskey config server set --workspace-id WORKSPACE_ID_VALUE
```

### Card Reader Configuration

Card reader configuration is used to configure how the Reader Driver communicates with cards or reads and interprets card identifiers from supported card readers. The available configuration options depend on the card reader type and the card technology used in the deployment.

#### rfIDEAS Card Reader Configuration

For rfIDEAS card readers, the card identifier, also referred to as the UID, can be read with or without byte padding.

By default, byte padding is disabled. When byte padding is disabled, if the card UID is 26 bits, the Reader Driver drops the ending 2 bits and reads the actual 3-byte UID.

When byte padding is enabled, the full 26-bit UID value is read.

Use the following command to enable or disable UID byte padding for rfIDEAS card readers:

```shell
accesskey config rfideas uid byte-padding ON|OFF
```

Examples:

```shell
accesskey config rfideas uid byte-padding ON
accesskey config rfideas uid byte-padding OFF
```

#### NFC Card Reader Configuration

Reader Configuration

NFC reader configuration is used to control how the Reader Driver communicates with the NFC reader device itself.

By default, NFC readers may be handled by the operating system through the standard CCID driver stack which provides the PCSC API for IDmelon service. In some deployments, the Reader Driver must communicate directly with the NFC reader to prevent Windows or other system components from detecting or interacting with the tapped card.

Use the following command to enable or disable the CCID driver mode for NFC readers:

```shell
accesskey config nfc reader ccid-driver set enabled|disabled
```

Examples:

```shell
accesskey config nfc reader ccid-driver set enabled
accesskey config nfc reader ccid-driver set disabled
```

> The CCID driver is enabled by default.

When the CCID driver is enabled, the Reader Driver installs the WinUSB driver on the supported NFC reader and communicates with the reader directly through the USB protocol using CCID data formats.

This configuration is intended to prevent the operating system from detecting the tapped card, so only the IDmelon Reader Driver can communicate with the card without interruption.

The NFC readers that support WinUSB driver installation are limited. The Reader Driver installs the WinUSB driver using pre-built `.inf` and `.cat` files included with the service package. These driver package files are signed through Microsoft Partner Center.

---------

Card Configuration

For NFC card readers, the expected card behavior can be configured by defining one or more NFC card definitions. Each definition tells the Reader Driver how to detect the tapped card, which identifier to read from it, and which post-read format should be applied to the identifier value.

The default NFC card type is `CSN`. In this mode, the Reader Driver reads only the UID of the card by sending the general APDU command.

Each NFC card definition can contain up to three sections:

```text
CARD_TYPE-IDENTIFIER_TYPE-IDENTIFIER_FORMAT
```

The sections are:

- `CARD_TYPE`: Defines the expected card technology or protocol. This section is required.
- `IDENTIFIER_TYPE`: Defines which identifier should be read from the card. This section is optional.
- `IDENTIFIER_FORMAT`: Defines the format or decoding rule that should be applied after the identifier is read. This section is optional.

For example:

```text
SEOS-PACS-H10301
iCLASS_SE-PACS-H10301
iCLASS_SE-UID
FIDO
CSN
```

In these examples:

- `SEOS-PACS-H10301` means the card type is `SEOS`, the identifier type is `PACS`, and the identifier format is `H10301`.
- `iCLASS_SE-PACS-H10301` means the card type is `iCLASS_SE`, the identifier type is `PACS`, and the identifier format is `H10301`.
- `iCLASS_SE-UID` means the card type is `iCLASS_SE` and the identifier type is `UID`.
- `FIDO` means the card type is `FIDO`, with no identifier type or identifier format.
- `CSN` means the card type is `CSN`, with no identifier type or identifier format.

Supported card types include:

- `CSN`: Reads the card UID by sending the general APDU command.
- `FIDO`: Communicates with the card using the CTAP NFC protocol and treats the card as a FIDO2 security key.
- `SEOS`: Reads the PACS value from a Seos card in a secure session. For Seos cards, PACS is the constant identifier type.
- `iCLASS_SE`: Reads the configured identifier from an iCLASS SE card. Depending on the sub-configuration, the Reader Driver can read either `UID` or `PACS`.

Supported identifier types include:

- `UID`: Uses the card UID as the identifier.
- `PACS`: Uses the PACS value as the identifier.

If the selected card type does not support the configured identifier type, the CLI returns an error.

The identifier format defines the post-read processing that the Reader Driver applies to the value read from the card. For example, this can include removing parity bits or decoding the value based on a standardized card format.

The format code is optional and is only required when the selected identifier must be decoded using a specific card format.

Example format codes:

```text
H10301
H10302
H10304
```

Use the following command to configure the NFC card type definition:

```shell
accesskey config nfc card type set CARD_TYPE[-IDENTIFIER_TYPE[-IDENTIFIER_FORMAT]]
```

Examples:

```shell
accesskey config nfc card type set CSN
accesskey config nfc card type set FIDO
accesskey config nfc card type set SEOS-PACS-H10301
accesskey config nfc card type set iCLASS_SE-UID
accesskey config nfc card type set iCLASS_SE-PACS-H10301
```

##### NFC Card Type Priority

If a single card type is configured, the Reader Driver expects the tapped card to match that card type. If the tapped card is not the expected card type, the service does nothing.

The Reader Driver can also be configured with a sequence of card definitions. In this case, the order of the definitions determines the priority. The service checks the tapped card against each definition in order and uses the first matching definition.

Example priority sequence:

```text
FIDO, SEOS, iCLASS_SE-PACS-H10301, CSN
```

With this sequence, the Reader Driver behaves as follows:

1. First, it checks whether the tapped card is a FIDO card. If it is, the service communicates with the card using CTAP NFC and treats it as a FIDO2 security key.
2. If the card is not FIDO, it checks whether the card is a Seos card. If it is, the service reads the PACS value in a secure session and uses it as the card identifier.
3. If the card is not Seos, it checks whether the card is an iCLASS SE card. If it is, the service reads the PACS value instead of the UID and decodes it using the `H10301` format.
4. If none of the previous card types match, it falls back to `CSN` and reads the legacy UID from the card by sending the general APDU command.

This priority-based configuration is useful in environments where multiple card technologies are used and the Reader Driver must support more than one card type on the same workstation.

> Note: The FIDO card type will only work if the CCID driver is enabled and the plugged card reader CCID driver is supported in IDmelon Card Reader Driver

## Deployment

The IDmelon Reader Driver can be deployed manually or through Microsoft Intune. Both deployment methods support configuring the Reader Driver service after installation.

### Manual Deployment

In manual deployment, the Reader Driver installer is executed directly on the target Windows machine.

The Reader Driver can be configured by passing the same configuration commands to the installer file instead of using the `accesskey` command.

For example, the following server configuration commands:

```shell
accesskey config server set --api-url https://skm.sample.com/api/url
accesskey config server set --api-key API_KEY_VALUE
accesskey config server set --workspace-id WORKSPACE_ID_VALUE
```

Can be passed to the installer file during manual deployment using the same arguments:

```shell
IDmelonReaderDriverInstaller.exe config server set --api-url https://skm.sample.com/api/url
IDmelonReaderDriverInstaller.exe config server set --api-key API_KEY_VALUE
IDmelonReaderDriverInstaller.exe config server set --workspace-id WORKSPACE_ID_VALUE
```

> Replace `IDmelonReaderDriverInstaller.exe` with the actual installer file name used in your deployment package.

This approach allows administrators to install and configure the Reader Driver on a device without running the `accesskey` command separately.

### Intune Deployment

The Reader Driver can also be deployed using Microsoft Intune. In this deployment method, the installer package is uploaded to Intune, and a configuration script is executed after installation to configure the Reader Driver service.

A typical Intune deployment includes the following steps:

1. Prepare the Reader Driver installer package.
2. Create a configuration script that applies the required Reader Driver settings.
3. Package the installer and script as a Win32 app package.
4. Upload the package to Microsoft Intune.
5. Configure the install command to install the Reader Driver.
6. Configure the script execution command to apply the required service configuration.
7. Assign the application to the required users or devices.
8. Monitor the installation and configuration status from the Intune admin center.

#### Example Configuration Script

The following PowerShell script configures the Reader Driver server settings after installation:

```powershell
$AccessKeyPath = "C:\Program Files (x86)\IDmelon\Reader Driver\accesskey.exe"

& $AccessKeyPath config server set --api-url "https://skm.sample.com/api/url"
& $AccessKeyPath config server set --api-key "API_KEY_VALUE"
& $AccessKeyPath config server set --workspace-id "WORKSPACE_ID_VALUE"
```

If the deployment package uses the installer file to apply configuration commands, the script can call the installer file with the same configuration arguments:

```powershell
$InstallerPath = "PATH-TO-INSTALLER-FILE"

& $InstallerPath config server set --api-url "https://skm.sample.com/api/url"
& $InstallerPath config server set --api-key "API_KEY_VALUE"
& $InstallerPath config server set --workspace-id "WORKSPACE_ID_VALUE"
```

> Replace the paths, API URL, API key, and workspace ID with the values that match your environment.

#### Example Intune Install Command

When creating the Win32 app in Intune, use an install command that installs the Reader Driver silently. For example:

```shell
IDmelonReaderDriverInstaller.exe /S config server set --api-url "https://skm.sample.com/api/url"
```

#### Example Intune Configuration Command

After installation, run the configuration script to apply the required service settings:

```shell
powershell.exe -ExecutionPolicy Bypass -File .\Configure-IDmelonReaderDriver.ps1
```

#### Detection Rule

Configure an Intune detection rule to verify that the Reader Driver has been installed successfully. For example, the detection rule can check whether the Reader Driver installation directory or service exists on the device.

Example file-based detection path:

```text
C:\Program Files (x86)\IDmelon\Reader Driver\accesskey.exe
```

Alternatively, use a custom detection script to verify that the Reader Driver service exists and is installed correctly.

#### Notes

- Use the correct API URL for the target deployment environment.
- For Reader Driver version 3.13.0 and later, configure the API key if authenticated client is required for targeted workspace.
- Configure the workspace ID when the Reader Driver must target a specific workspace.
- Validate the deployment on a test device before assigning it to production users or devices.
