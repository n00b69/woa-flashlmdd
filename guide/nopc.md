<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Installing Windows without a PC

### Prerequisites
- A rooted LG V50 with LineageOS 20 or an SD card

- [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)

- [Modified TWRP](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/modded-twrp-v50-installer.zip)

- [LG V50 WinInstaller](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/FlashlmddWinInstaller.zip)

- [Windows on ARM image](https://arkt-7.github.io/woawin/)

- [WOA Helper app](https://github.com/n00b69/woa-helper/releases/tag/APK)

### Notes
> [!WARNING]  
> All your data will be erased! Back up now if needed.
> 
> DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help in the [Telegram chat](https://t.me/lgedevices).

> [!Important]
> This guide assumes you have already unlocked your bootloader and are already rooted, if this is not the case, you'll still need a PC to do that.

### Flash the modified TWRP
- Download **Magisk.apk** on your phone and rename it to **Magisk.zip**. This is necessary because flashing TWRP will remove your root.
- If you are not using LineageOS 20, copy it to your SD card. TWRP cannot decrypt the internal storage of other ROMs.
- In **Magisk**, select the **modded-twrp-v50-installer.zip** and flash it.
- Return to the main menu, press the rotating arrow icon in the top right, and press `Reboot Recovery`.

### Rerooting your device
- Once booted into TWRP, select **Install** and locate **Magisk.zip** and flash it.
- Reboot to Android, open the Magisk app, and follow the steps on the screen to complete the root process.

#### Reboot into TWRP
- Return to the Magisk main menu, press the rotating arrow icon in the top right, and press `Reboot Recovery`.

#### Opening TWRP terminal
- Once booted into TWRP press the **Advanced** button on the bottom right of the screen, then press **Terminal**.
- Run all future commands in this terminal

#### Unmount data
> Ignore any possible errors and continue
```cmd
umount /dev/block/by-name/userdata
```

### Fixing the GPT
> If you do not do this, Windows may break your device
```cmd
fixgpt
```

#### Preparing for partitioning
> [!Note]
> If at any time **parted** asks you if you want to continue, or if you want to cancel something, type **yes** or **ignore**
>
> Be very careful with writing in parted. It is not possible to use backspace to fix a typo. If at any point you make any mistakes, use the **print** command to check if any changes were made
```cmd
parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, userdata should be the last partition in the list.
```cmd
print
```

#### Removing userdata
> Replace **$** with the number of the **userdata** partition, which should be **30**
>
> If you have a **grow** partition, remove it as well
```cmd
rm $
```

#### Recreating userdata
> Replace **17.7GB** with the former start value of **userdata** which we just deleted
>
> Replace **64GB** with the end value you want **userdata** to have. In this example your available usable space in Android will be 64GB-17.7GB = **47GB**
```cmd
mkpart userdata ext4 17.7GB 64GB
```

#### Creating ESP partition
> Replace **64GB** with the end value of **userdata**
>
> Replace **64.35GB** with the value you used before, adding **0.35GB** to it
```cmd
mkpart esp fat32 64GB 64.35GB
```

#### Creating Windows partition
> Replace **64.35GB** with the end value of **esp**
```cmd
mkpart win ntfs 64.35GB 126GB
```

#### Making ESP bootable
> Use `print` to see all partitions. Replace "$" with your ESP partition number, which should be **31**
```cmd
set $ esp on
```

#### Exit parted
```cmd
quit
```

### Formatting data and rebooting
- Format all data in TWRP, or Android will not boot.
- ( Go to Wipe > Format data > type `yes` ).
- Press the reboot button to reboot into Android.
> [!Note]
> If Android does not start after +- 10 minutes, reboot back into stock recovery and perform a factory reset there.

### Preparing necessary files
- Download the Windows image and make sure it remains in the `Download` folder of your **internal storage**.
> [!Important]
> For performance reasons, it is recommended to use Windows 11 24H2 (builds that start with 261XX, such as 26100.2454)

> If TWRP is not able to read/decrypt your internal storage, create a folder on your **micro SD card** named `WOA` and put the **.esd** file in there.
- Download **WinInstaller.zip** and keep it in the `Download` folder as well.
- Download and install the [WOA Helper app](https://github.com/n00b69/woa-helper/releases/tag/APK), then open it and grant it root access. Do not do anything else inside the app yet.

### Booting into TWRP
- Press the `Reboot to Recovery` button in the top right of Magisk again.
- Once booted into TWRP, enter your password if it asks you to.

### Flashing WinInstaller
- In TWRP, select **Install** and then locate **WinInstaller.zip** and flash it.
- Once you're given the option to reboot, do so.
> [!Note]
> Wait untill all processes complete and your device boots into Windows. This will take around 15-20 minutes.

> [!Tip]
> If you wish to skip the Microsoft Account login, press the **I don't have internet** button in the WiFi page, then when prompted, press the **Continue with limited setup** button.

#### Booting to Android
- Run the **Android** shortcut on your desktop (you can also pin it to your start menu / taskbar for ease of access).

#### Booting to Windows
> If it says "UEFI NOT FOUND", download the [UEFI image](https://github.com/n00b69/woa-flashlmdd/releases/tag/UEFI) and place it in the `UEFI` folder in your internal storage.
- Press **QUICKBOOT TO WINDOWS** inside the WOA Helper app, or use the newly created toggle in your quick settings panel.

## Finished!

























