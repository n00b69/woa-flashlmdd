<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Installing Windows

### Prerequisites
- [Windows on ARM image](https://arkt-7.github.io/woawin/)
  
- [Drivers](https://github.com/n00b69/woa-flashlmdd/releases/tag/Drivers)

- [Mass storage image](https://github.com/n00b69/woa-mh2lm5g/releases/download/Files/massstorage.img)

- [Qfil](https://github.com/n00b69/woa-flashlmdd/releases/tag/Qfil)

### Reboot to fastboot mode
- With the device turned off, hold the **volume down** button, then plug the cable in.
- If the phone in device manager is called **Android** and has a ⚠️ yellow warning triangle, you need to install fastboot drivers before you can continue.
- To install fastboot drivers, extract the contents of **QUD.zip** somewhere, right click on **Android**, click on **Update driver** and **Browse my computer for drivers**, then find and select the **QUD** folder.

#### Boot to the mass storage mode image
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
```diskpart
assign letter x
```

#### Select the ESP volume of the phone
> Use `list volume` to find it, replace `$` with the actual number of **ESPFLASH**
```diskpart
select volume $
```

#### Assign the letter Y
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

### Boot into any custom recovery
> Reboot your phone by holding **volume down** + **power** until it shows the LG logo, then release the buttons. After this boot to any custom recovery again.

#### Push parted
> Make sure your CMD is opened in platform-tools
```cmd
adb push parted /cache/ && adb shell "chmod 755 /cache/parted" && adb shell /cache/parted /dev/block/sda
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

### Reboot to EDL
> If you didn't flash the engineering ABL on the previous page, you can skip this step and the next one and simply reboot your device
- Open **Device Manager** on your PC
- Hold **volume down** + **power**.
- After the LG logo appears, while still holding **volume down** + **power**, start rapidly pressing the **volume up** button.
- Keep doing this until you hear a USB connection sound on your PC, or when **Qualcomm HS-USB QDLoader 9008** appears in the **Ports (COM & LPT)** category of Device Manager.

#### Flashing stock ABL
> Or your IMEI won't work
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **abl_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **abl_a** file in `C:\Users\YOURNAME\AppData\Roaming\Qualcomm\QFIL\COMPORT_#\`
- Do the same thing for **abl_b**.

#### Reboot back to Android
- Hold **volume down** + **power** until it shows the LG logo, then release the buttons.

## [Last step: Setting up dualboot](4-dualboot.md)























