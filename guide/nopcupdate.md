<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Updating drivers without a PC

### Prerequisites
- A rooted LG V50 with LineageOS 20 or an SD card

- [LG V50 DriverUpdater (LOS20)](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/FlashlmddDriverUpdater.zip) or [LG V50 DriverUpdater (SD card)](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/FlashlmddDriverUpdaterSDCARD.zip) (If you are using an SD card)

- [Modified TWRP](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/modded-twrp-v50-installer.zip)

- [WOA Helper app](https://github.com/Marius586/WoA-Helper-update/releases/tag/WOA)

### Notes
> [!WARNING]  
> 
> DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help in the [Telegram chat](https://t.me/woahelperchat).

> [!Important]
> This guide assumes you have already installed Windows. If you haven't, use the [installation guide](nopc.md) instead.

### Preparing necessary files
- Download **FlashlmddDriverUpdater.zip** and keep it in your **internal storage** (if you are using LineageOS 20), else, put it in your **SD card**.

### Flash the modified TWRP
> Or boot into TWRP if you've already installed it.
- In **Magisk**, select the **modded-twrp-v50-installer.zip** and flash it.
- Return to the main menu, press the rotating arrow icon in the top right, and press `Reboot Recovery`.

### Flashing DriverUpdater
- In TWRP, select **Install** and then locate **FlashlmddDriverUpdater.zip** and flash it.
- Once you're given the option to reboot, do so.
> [!Note]
> Wait untill all processes complete and your device boots back into Windows. This will take around 20-30 minutes.

## Finished!
