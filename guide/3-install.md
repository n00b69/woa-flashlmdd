<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Installing Windows

### Prerequisites
- [Modded TWRP](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/modded-twrp-v50.img)

- [Windows on ARM image](https://arkt-7.github.io/woawin/)
  
- [Drivers](https://github.com/n00b69/woa-flashlmdd/releases/tag/Drivers)

- [UEFI image](https://github.com/n00b69/woa-flashlmdd/releases/tag/UEFI)

### Reboot into fastboot mode
- With the device turned off, hold the **volume down** button, then plug the cable in.

### Boot modified TWRP recovery
> Replace `path\to\modded-twrp-v50.img` with the actual path of the image
```cmd
fastboot boot path\to\modded-twrp-v50.img
```

#### Execute the msc script
```cmd
adb shell msc
```

> [!Note]
> If you are facing issues (e.g your device does not enter mass storage mode), follow [the steps described in this guide](https://github.com/n00b69/woa-flashlmdd/blob/main/guide/troubleshooting.md#mass-storage-mode-does-not-work) for alternative ways of entering mass storage mode.

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
> [!Important]
> Do not install, or update to, Windows 11 **24H2 26100.7XXX** / **25H2 26200.7XXX** or higher! You will not be able to boot into these builds due to a BSoD issue!
>
> Builds prior to **26X00.7XXX** are safe to use.

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

### Rebooting into fastboot mode
```cmd
adb reboot bootloader
```

### Boot into the UEFI
> Replace `path\to\flashlmdd-uefi.img` with the actual path of the image
```cmd
fastboot boot path\to\flashlmdd-uefi.img
```

> After around 15 minutes your screen will go black. When it does, force reboot your device using **volume down** + **power**, then boot back into fastboot mode and boot the UEFI image one more time and wait until you see the language selection screen in Windows.

> You may notice that everything feels very slow. This is normal and will be fixed in the next step.

### Enabling GPU
> Currently, GPU drivers are not installed when you first boot Windows. To fix this, we do the following
- Open **Device Manager**, click on **Display Adapters**, then double click on **Microsoft Basic Display Adapter**.
- Press `Update Driver` > `Browse my computer for drivers` > `Let me pick from a list of available drivers on my computer`, then select **Qualcomm(R) Adreno(TM) 640 GPU** and press `Next`.
- Your screen should go black for a few seconds, after which you'll have succesfully installed the GPU drivers.

#### Reboot back into Android
- Run the **Android** shortcut on your desktop (you can also pin it to your start menu / taskbar for ease of access)

## [Last step: Setting up dualboot](4-dualboot.md)























