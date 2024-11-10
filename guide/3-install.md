<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Installing Windows

### Prerequisites
- [Mass storage image](https://github.com/n00b69/woa-mh2lm5g/releases/download/Files/massstorage.img)

- [Windows on ARM image](https://arkt-7.github.io/woawin/)
  
- [Drivers](https://github.com/n00b69/woa-flashlmdd/releases/tag/Drivers)

- [UEFI image](https://github.com/n00b69/woa-flashlmdd/releases/tag/UEFI)

### Reboot into fastboot mode
- With the device turned off, hold the **volume down** button, then plug the cable in.

#### Boot into the mass storage mode image
> Replace `path\to\massstorage.img` with the actual path of the image
>
> If popups show up telling you to format the disks, ignore or close them
```cmd
fastboot boot path\to\massstorage.img
```

### Diskpart
> [!WARNING]
> DO NOT ERASE, CREATE OR OTHERWISE MODIFY ANY PARTITION WHILE IN DISKPART!!!! THIS CAN ERASE ALL OF YOUR UFS OR PREVENT YOU FROM BOOTING TO FASTBOOT!!!! THIS MEANS THAT YOUR DEVICE WILL BE PERMANENTLY BRICKED WITH NO SOLUTION! (except for flashing it with EDL, which is complicated)
```cmd
diskpart
```

#### Select the Windows volume of the phone
> Use `list volume` to find it, replace `$` with the actual number of **WINFLASH**
```diskpart
select volume $
```

#### Assign the letter X
> If letter `X` cannot be assigned, use letter `A` instead, and replace the letter `X` in future commands with the letter `A`
```diskpart
assign letter x
```

#### Select the ESP volume of the phone
> Use `list volume` to find it, replace `$` with the actual number of **ESPFLASH**
```diskpart
select volume $
```

#### Assign the letter Y
> If letter `Y` cannot be assigned, use letter `B` instead, and replace the letter `Y` in future commands with the letter `B`
```diskpart
assign letter y
```

#### Exit diskpart
```cmd
exit
```

### Installing Windows
> [!Warning]
> DO NOT USE 24H2!!!

> Replace `path\to\install.esd` with the actual path of install.esd (it may also be named install.wim or 22631.2861.XXXXXXX.esd)

```cmd
dism /apply-image /ImageFile:path\to\install.esd /index:6 /ApplyDir:X:\
```

> If you get `Error 87`, check the index of your image with `dism /get-imageinfo /ImageFile:path\to\install.esd`, then replace `index:6` with the actual index number of **Windows 11 Pro** in your image

### Copying your boot.img into Windows
- Drag and drop the **rooted_boot.img** from the **platform-tools** folder into the **WINFLASH** disk in Windows Explorer, then rename it to **boot.img**

### Installing drivers
- Unpack the driver archive, then open the `OfflineUpdater.cmd` file (if an error shows up, run `OfflineUpdaterFix.cmd` instead)

> If it asks you to enter a letter, enter the drive letter of **WINFLASH** (which should be **X**), then press enter

> [!important]
> After booting into Windows later in the guide, reboot back into mass storage mode and open **OfflineUpdater.cmd** again to reinstall drivers. GPU drivers do not get installed properly otherwise.
>
> If you get an "Registry hive needs transaction logs" error when doing so, please join the [Telegram chat](https://t.me/woahelperchat) and write `#hive` for instructions on how to solve this.
  
#### Create the Windows bootloader files
> If any error shows up, such as "Failure when attempting to copy boot files", open `diskpart` again and assign any new letter to **ESPFLASH**, then replace the letter `Y` in the next commands with the letter that you just added.
>
> Reboot your PC if no letters are available to be assigned.
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

#### Enabling test signing
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" testsigning on
```

#### Disabling recovery
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" recoveryenabled no
```

#### Disabling integrity checks
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" nointegritychecks on
```

#### Reboot to fastboot mode
- Reboot your phone by holding **volume down** + **power** until it shows the LG logo, then release the buttons.
- After it has booted, unplug the cable and power it off.
- Once the device has turned off, hold the **volume down** button, then plug the cable back in.

#### Boot into the modded TWRP
> Replace `path\to\modded-twrp-v50.img` with the actual path of the provided TWRP image
>
> After booting into TWRP, leave the device on the main screen. You can press the power button to turn the display off, if you want
```cmd
fastboot boot path\to\modded-twrp-v50.img
```

### Running parted
```cmd
adb shell parted /dev/block/sda
```

#### Making ESP bootable
> Use `print` to see all partitions. Replace "$" with your ESP partition number, which should be 31
```cmd
set $ esp on
```

#### Exit parted
```cmd
quit
```

### Rebooting into fastboot mode
```cmd
adb reboot bootloader
```

### Boot into the UEFI
> Replace `path\to\flashlmdd-uefi.img` with the actual path of the image
```cmd
fastboot boot path\to\flashlmdd-uefi.img
```

### Reboot into Android
Your device should reboot by itself after +- 10 minutes of waiting, after which you will be booted into Android, for the last step.

## [Last step: Setting up dualboot](4-dualboot.md)























