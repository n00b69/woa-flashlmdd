<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Updating drivers

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
  
- [Drivers](https://github.com/n00b69/woa-betalm/releases/tag/Drivers)

- [UEFI image](https://github.com/n00b69/woa-betalm/releases/tag/UEFI)

### Boot to the UEFI
> Replace **<path\to\msc.img>** with the actual path of the UEFI image
```cmd
fastboot boot <path\to\msc.img>
```

#### Enabling mass storage mode
> Once booted into the UEFI, use the volume buttons to navigate the menu and the power button to confirm
- Select **UEFI Boot Menu**.
- Select **USB Attached SCSI (UAS) Storage**.
- Press the **power** button twice to confirm.

### Diskpart
```cmd
diskpart
```

#### Select the phone's Windows volume
> Use `list volume` to find it, it should be named **WINFLASH**
```diskpart
select volume <number>
```

#### Assign the letter x
```diskpart
assign letter x
```

#### Exit diskpart:
```diskpart
exit
```

### Installing Drivers
> Unpack the driver archive, then open the `OfflineUpdater.cmd` file

> Enter the drive letter of `WINFLASH`, which should be X, then press enter

#### Reboot your device
> Once the drivers have finished installing

## Finished!

















