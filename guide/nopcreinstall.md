<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Reinstalling Windows without a PC

### Prerequisites
- A rooted LG V50 with LineageOS 20 or an SD card

- [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)

- [Modified TWRP](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/modded-twrp-v50-installer.zip)

- [LG V50 WinInstaller](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/FlashlmddWinInstaller.zip)

- [Windows on ARM image](https://arkt-7.github.io/woawin/)

- [WOA Helper app](https://github.com/n00b69/woa-helper/releases/tag/APK)

### Flash the modified TWRP
> [!Important]
> If you have already installed the modified TWRP, skip to "Preparing necessary files"
- Download **Magisk.apk** on your phone and rename it to **Magisk.zip**. This is necessary because flashing TWRP will remove your root.
- If you are not using LineageOS 20, copy it to your SD card. TWRP cannot decrypt the internal storage of other ROMs.
- In **Magisk**, select the **modded-twrp-v50-installer.zip** and flash it.
- Return to the main menu, press the rotating arrow icon in the top right, and press `Reboot Recovery`.

### Rerooting your device
- Once booted into TWRP, select **Install** and locate **Magisk.zip** and flash it.
- Reboot to Android, open the Magisk app, and follow the steps on the screen to complete the root process.

### Preparing necessary files
- Download the Windows image and make sure it remains in the `Download` folder of your **internal storage**.
- Download **WinInstaller.zip** and keep it in the `Download` folder as well.
> [!Important]
> For performance reasons, it is recommended to use Windows 11 24H2 (builds that start with 261XX, such as 26100.2454)

> If TWRP is not able to read/decrypt your internal storage, create a folder on your **micro SD card** named `WOA` and put the **.esd** file in there.

#### Reboot into TWRP
- Return to the Magisk main menu, press the rotating arrow icon in the top right, and press `Reboot Recovery`.

#### Opening TWRP terminal
> Enter your password in TWRP, if it asks you to.
- Press the **Advanced** button on the bottom right of the screen, then press **Terminal**.

### Formatting Windows
```cmd
format
```

### Flashing WinInstaller
- In TWRP, select **Install** and then locate **WinInstaller.zip** and flash it.
- Once you're given the option to reboot, do so.
> [!Note]
> Wait until all processes complete and your device boots into Windows. This will take around 15-20 minutes.

> [!Tip]
> If you wish to skip the Microsoft Account login, press the **I don't have internet** button in the WiFi page, then when prompted, press the **Continue with limited setup** button.

#### Booting to Android
- Run the **Android** shortcut on your desktop (you can also pin it to your start menu / taskbar for ease of access).

#### Booting to Windows
> If it says "UEFI NOT FOUND", download the [UEFI image](https://github.com/n00b69/woa-flashlmdd/releases/tag/UEFI) and place it in the `UEFI` folder in your internal storage.
- Press **QUICKBOOT TO WINDOWS** inside the WOA Helper app, or use the newly created toggle in your quick settings panel.

## Finished!

























