# Windows Autounattend Installation Script

## Overview

This repository contains an Autounattend.xml script designed for unattended installations of Windows. The script automates the setup process, providing configuration details for various installation phases, including Windows PE, specialize, and OOBE.

Prerequisites

Before you begin, ensure you have the following:

    A Windows 10 ISO file.
    A USB drive with sufficient space (at least 8GB is recommended).
    Rufus or another tool for creating bootable USB drives.

Creating Your Own Windows 10 USB

Follow these steps to create a Windows 10 USB drive with the unattended installation setup:

    Download the Windows 10 ISO:
        Obtain a Windows 10 ISO file from the official Microsoft website.

    Create a Bootable USB Drive:
        Use Rufus or another tool to create a bootable USB drive from the Windows 10 ISO file.

    Copy Autounattend.xml and Autorun.inf:
        Copy the provided Autounattend.xml and Autorun.inf files from this repository to the root directory of the USB drive.

    Customize Autounattend.xml (Optional):
        If needed, open Autounattend.xml in a text editor and customize settings such as the product key or user information.

    Eject and Reinsert the USB Drive:
        Eject the USB drive safely and then reinsert it into the target system.

    Install Windows 10:
        Boot the target system from the USB drive to start the unattended installation process.

## Usage

1. **Customization:**
    - Open the `Autounattend.xml` file in a text editor.
    - Fill in the required information such as product key and user password.
    - Customize other settings based on your deployment needs, such as disk configurations, user details, and regional settings.

2. **Generate ISO:**
    - Use the script in conjunction with the Windows Assessment and Deployment Kit (ADK) or another deployment tool to create a customized Windows installation ISO.

3. **Deployment:**
    - Boot from the customized ISO on target machines to initiate unattended installations.

## Script Sections

### 1. `windowsPE`
   - Configures settings for the Windows Preinstallation Environment (WinPE).
   - Specifies language and disk configurations.

### 2. `specialize`
   - Customizes settings after installation but before the first user logon.
   - Sets up OEM information, computer name, user details, and time zone.

### 3. `oobeSystem`
   - Configures settings for the Out-of-Box Experience (OOBE) phase.
   - Customizes the OOBE phase by hiding certain screens and configuring user accounts.

## Important Notes

- **Customization:**
    - Replace placeholder values (e.g., product key) with your specific details.
    - Tailor the script according to your deployment requirements.

- **Validation:**
    - Use the Windows System Image Manager (SIM) tool to validate and test your Autounattend.xml file.

## Example Deployment

```bash
# Mount the Windows installation ISO
$ mkdir /mnt/iso
$ sudo mount -o loop path/to/windows.iso /mnt/iso

# Copy the Autounattend.xml file to the ISO
$ cp Autounattend.xml /mnt/iso/sources/

# Recreate the ISO with the updated Autounattend.xml
$ genisoimage -o custom_windows.iso -b boot/etfsboot.com -no-emul-boot -boot-load-seg 0x07 -boot-load-size 8 -iso-level 2 -udf -joliet -D -N -relaxed-filenames -allow-lowercase -allow-multidot -l -r -J -jcharset utf-8 -hide-joliet boot.catalog -V WINDOWS_INSTALL /mnt/iso

# Burn the custom ISO to a USB drive or DVD
$ dd if=custom_windows.iso of=/dev/sdX bs=4M status=progress
