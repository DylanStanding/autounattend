# Windows Autounattend Installation Script

## Overview

This repository contains an Autounattend.xml script designed for unattended installations of Windows. The script automates the setup process, providing configuration details for various installation phases, including Windows PE, specialize, and OOBE.

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
