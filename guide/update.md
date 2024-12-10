<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Updating drivers

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Modded TWRP](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/modded-twrp-v50.img)

- [Drivers](https://github.com/n00b69/woa-flashlmdd/releases/tag/Drivers)

- [UEFI](https://github.com/n00b69/woa-flashlmdd/releases/tag/UEFI)

### Reboot into fastboot mode
> If you don't have access to fastboot, use the instructions in the [partitioning guide](1-partition.md) to flash the engineering ABL.
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
> **WINFLASH** should automatically appear in Windows Explorer. If it does, skip to the "Installing drivers" step, else continue with the "Diskpart" steps.

> [!Note]
> If you are facing issues (e.g your device does not enter mass storage mode), follow [the steps described in this guide](https://github.com/n00b69/woa-mh2lm5g/blob/main/guide/troubleshooting.md#mass-storage-mode-does-not-work) for alternative ways of entering mass storage mode.

### Diskpart
```cmd
diskpart
```

#### Select the phone's Windows volume
> Use `list volume` to find it, replace `$` with the actual number of **WINFLASH**
```diskpart
select volume $
```

#### Assign the letter x
```diskpart
assign letter x
```

#### Exit diskpart
```diskpart
exit
```

### Installing Drivers
> [!Note]
> This process will take +- 20 minutes. Do not worry, this is normal.

> Unpack the driver archive, then open the `OfflineUpdater.cmd` file (if an error shows up, run `OfflineUpdaterFix.cmd` instead)

> If it asks you to enter a letter, enter the drive letter of **WINFLASH** (which should be **X**), then press enter

### Reboot your device
> Force reboot your phone by holding **volume down** + **power** until you see the LG logo.
>
> If you end up in Android instead of Windows, simply use the WOA Helper app to switch back.

## Finished!

















