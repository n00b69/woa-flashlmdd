<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Updating drivers

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Qfil](https://github.com/n00b69/woa-flashlmdd/releases/tag/Qfil)

- [Engineering ABL](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/engabl_ab.bin)
  
- [Drivers](https://github.com/n00b69/woa-flashlmdd/releases/tag/Drivers)

- [Mass storage image](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/massstorage.img)

### Boot to EDL
- Open **Device Manager** on your PC
- With the phone turned off, hold **volume down** + **power**.
- After the screen turns dark, while still holding **volume down** + **power**, start rapidly pressing the **volume up** button.
- Keep doing this until you see **QDLoader 9008** or **QUSB_BULK** in the Device Manager on your PC.
- If the device has a ⚠️ yellow warning triangle, you need to install EDL drivers before you can continue to the next step.

#### Setting up Qfil
- Open **Qfil**.
- In "Select Build Type", select **flat build**.
- In "Select programmer", select the downloaded firehose.
- In Configuration, make sure the "Device Type" is set to **UFS**.

### Flashing engineering ABL
> Make sure you've made abl backups before
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **abl_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **engabl_ab.bin** file.
- Do the same thing for **abl_b**.

### Reboot to fastboot mode
- Force reboot your phone by holding **volume down** + **power** until you see the LG logo.
- With the device turned off, hold the **volume down** button, then plug the cable in.
- If the phone in device manager is called **Android** and has a ⚠️ yellow warning triangle, you need to install fastboot drivers before you can continue.

#### Boot to the mass storage mode image
> Replace `path\to\massstorage.img` with the actual path of the image
>
> If popups show up telling you to format the disks, ignore or close them
```cmd
fastboot boot path\to\massstorage.img
```

> [!Note]
> After 1-2 minutes **WINMH2LM5G** should automatically appear in Windows Explorer. If it does, skip to the "Installing drivers" step, else continue with the "Diskpart" steps.

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

### Reboot back to edl
> To restore your original abl
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **abl_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **abl_a.bin** file you've hopefully backed up before that is located in `C:\Users\YOURNAME\AppData\Roaming\Qualcomm\QFIL\COMPORT_#\`
- Do the same thing for **abl_b**.

### Reboot your device
> Force reboot your phone by holding **volume down** + **power** until you see the LG logo.
>
> If you end up in Android instead of Windows, simply use the WOA Helper app to switch back.

## Finished!

















