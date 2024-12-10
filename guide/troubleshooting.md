<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Troubleshooting Issues
> Below you will find a list of common problems and their solutions

## Mass storage mode does not work
> This happens for some users, it is unknown why it happens, to bypass this, do the following:
- Reboot back into the modified TWRP and run `adb shell parted /dev/block/sda`, then run `print`.
- Run `set $ esp off` and `set $ msftdata on`, replacing **$** with the actual number of the esp partition (should be 31).
- Boot into fastboot mode using `adb reboot bootloader`, then download [**LGG8XMassStorageBoot.img**](https://github.com/n00b69/woa-mh2lm/releases/download/Files/LGG8XMassStorageBoot.img) and boot into it using `fastboot boot path\to\LGG8XMassStorageBoot.img`.
- After you finish the installation guide, before booting the Windows UEFI, return to TWRP, reopen **parted** and run `set $ esp on` and `set $ msftdata off`.

##### Finished!

## Device is not recognized in fastboot or recovery
> This likely means you don't have (proper) USB drivers installed
- Download [QUD.zip](https://github.com/n00b69/woa-betalm/releases/download/Qfil/QUD.zip) here and extract it.
- Open Device Manager and find an unknown device or device with errors that may be called **Android**, **ADB Interface**, or **QUSB_BULK**.
- Right click this devjce, select "Update Drivers" > "Browse files", then select the **QUD folder** you extracted before.

##### Finished!

## Cannot mount Windows in Android
If mounting Windows produces an empty folder, you either don't have Windows installed, or your rom does not have mount support.

Solution: Install **LineageOS 20**

##### Finished!

## Cannot write to Windows in Android
> This is caused by shutting down Windows instead of restarting it.
- To solve this, boot to Windows and then press "restart", then as the screen shuts off boot to fastboot and flash your Android boot image.
- Or, disable hibernation in Windows using [this](https://github.com/n00b69/woa-beryllium/releases/tag/1.0) script.
> Alternatively, if you have already set up the Switch to Android app, simply use this to switch to Android.

##### Finished!

## USB does not work
Enable USB host mode using the [additional materials guide](materials.md#toggling-usb-host-mode).

##### Finished!

## Error: 3 The system cannot find the path specified.
This error usually means that you are trying to install Windows on a disk that already has Windows installed. To solve this issue, format the disk in Windows Explorer and try again.

##### Finished!

## 0xc000021a BSOD
This usually means that winlogon.exe has failed, and you may need to reapply the Windows image.

##### Finished!

## The computer restarted unexpectedly or encountered an unexpected error
If you stumble upon this error, you will need to [reinstall Windows](reinstall.md).

##### Finished!

## INACCESSIBLE_BOOT_DEVICE BSOD
This Blue Screen of Death likely means some broken driver installation. To fix this, [reinstall Windows](reinstall.md).

##### Finished!












