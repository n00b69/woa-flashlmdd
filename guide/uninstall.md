<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Uninstalling Windows

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Qfil](https://github.com/n00b69/woa-flashlmdd/releases/tag/Qfil)

- [Parted script](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/parted)
  
- Any custom recovery
  
- Boot backups

### Reboot to a modded recovery
> Which should be accessible by holding the **volume up** + **power** buttons, or with the **Reboot to recovery** button in Magisk

### Running parted
> Download the parted file and move it in the platform-tools folder, then run
```cmd
adb push parted /cache/ && adb shell "chmod 755 /cache/parted" && adb shell /cache/parted /dev/block/sda
```

#### Delete Windows Partition
> Use `print all` to make sure that partition 32 is Windows
```sh
rm 32
```

#### Delete ESP Partition
> Use `print all` to make sure that partition 31 is ESP
```sh
rm 31
```

#### Resize userdata Partition
> Use `print all` to make sure that partition 30 is userdata
```sh
resizepart 30
126GB
```

#### Exit Parted
```sh
quit
```

### Reboot your phone
```cmd
adb reboot
```
> [!note]
> If your last boot was in Windows, you may find yourself booting the UEFI instead of Android. If this is the case, you'll need to do the additional steps below.

### Boot to EDL
- Open **Device Manager** on your PC
- With the phone turned off, hold **volume down** + **power**.
- After the LG logo appears, while still holding **volume down** + **power**, start rapidly pressing the **volume up** button.
- Keep doing this until you see **QDLoader 9008** or **QUSB_BULK** in the Device Manager on your PC.
- If the device has a ⚠️ yellow warning triangle, you need to install EDL drivers before you can continue to the next step.

#### Setting up Qfil
- Open **Qfil**.
- In "Select Build Type", select **flat build**.
- In "Select programmer", select the downloaded firehose.
- In "Configuration", make sure the "Device Type" is set to **UFS**.

#### Flashing stock boot images
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **boot_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **boot_a** backup you made earlier when installing Windows (which should be in `C:\Users\YOURNAME\AppData\Roaming\Qualcomm\QFIL\COMPORT_#\`)
- Do the same thing for **boot_b**.

### Reboot your phone
- Hold **volume down** + **power** until it shows the LG logo, then release the buttons.

## Finished!



















