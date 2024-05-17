<img align="right" src="https://github.com/n00b69/woa-betalm/blob/main/betalm.png" width="350" alt="Windows 11 running on betalm">

# Running Windows on the LG G8s

## Uninstalling Windows

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Qfil](https://github.com/n00b69/woa-betalm/releases/tag/Qfil)
  
- [Modded TWRP](https://github.com/n00b69/woa-betalm/releases/download/Files/moddedg8s.img)
  
- Boot backups

### Reboot to fastboot mode
- Reboot your phone.
- After it has booted, unplug the cable and power it off.
- Once the device has turned off, hold the **volume down** button, then plug the cable back in.

#### Boot into TWRP
> Replace `path\to\moddedg8s.img` with the actual path of the provided TWRP image
>
> After booting into TWRP, leave the device on the main screen. You can press the power button to turn the display off, if you want
```cmd
fastboot boot path\to\moddedg8s.img
```

### Running parted
> Replug the cable if it says "no devices/emulators found"
```cmd
adb shell parted /dev/block/sda
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
- After the screen turns dark, while still holding **volume down** + **power**, start rapidly pressing the **volume up** button.
- Keep doing this until you see **QDLoader 9008** or **QUSB_BULK** in the Device Manager on your PC.
- If the device has a ⚠️ yellow warning triangle, you need to install EDL drivers before you can continue to the next step.

#### Setting up Qfil
- Open **Qfil**.
- In "Select Build Type", select **flat build**.
- In "Select programmer", select the downloaded firehose.
- In Configuration, make sure the "Device Type" is set to **UFS**.

#### Flashing engineering ABL
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **boot_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **boot_a** backup you made earlier when installing Windows (which should be in `C:\users\name\AppData\roaming\qualcomm\qfil\comportno\`)
- Do the same thing for **boot_b**.

### Reboot your phone
- Hold **volume down** + **power** until it shows the LG G8s logo, then release the buttons.

## Finished!



















