<img align="right" src="https://github.com/n00b69/woa-betalm/blob/main/betalm.png" width="350" alt="Windows 11 running on betalm">

# Running Windows on the LG G8s

## Partitioning your device

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Qfil](https://github.com/n00b69/woa-betalm/releases/tag/Qfil)

- [Engineering ABL](https://github.com/n00b69/woa-betalm/releases/download/Files/engabl_ab.bin)
  
- [Modded TWRP](https://github.com/n00b69/woa-betalm/releases/download/Files/moddedg8s.img)

### Notes
> [!WARNING]  
> 
> Do not run the same command twice unless specified.
>  
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!
>
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/winong8x).

### Backing up partitions
> If you don't do this and mess something up, you're on your own

#### Boot to EDL
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

#### Backing up your partitions
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **laf_a** > **Manage Partition Data** and press **Read Data**.
- Do the same thing for **laf_b**, **boot_a**, **boot_b**, **abl_a**, **abl_b**, **aop_a**, **aop_b**, **xbl_a**, **xbl_b**, **fsg**, **fsc**, **modemst1**, **modemst2**, **modem_a**, **modem_b**

> [!Important]
> Navigate to `C:\Users\YOURNAME\AppData\Roaming\Qualcomm\QFIL\COMPORT_#\` and rename the backed up partitions one by one as you back them up. Qfil does not name the backups, and if you don't rename them, it'll be impossible to figure out which files are which. You can restore them later with the **Load Image** function.

### Flashing engineering ABL
> If fastboot works on your phone, you can skip this step
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **abl_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **engabl_ab.bin** file.
- Do the same thing for **abl_b**.

#### Reboot to fastboot mode
- Reboot your phone.
- After it has booted, unplug the cable and power it off.
- Once the device has turned off, hold the **volume down** button, then plug the cable back in.
- If the phone in device manager is called **Android** and has a ⚠️ yellow warning triangle, you need to install fastboot drivers before you can continue.

#### Boot into TWRP
> Replace `path\to\moddedg8s.img` with the actual path of the provided TWRP image
>
> After booting into TWRP, leave the device on the main screen. You can press the power button to turn the display off, if you want
```cmd
fastboot boot path\to\moddedg8s.img
```

### Preparing for partitioning
> Replug the cable if it says "no devices/emulators found"
```cmd
adb shell parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, **userdata** should be the last partition in the list.
```cmd
print
```

#### Removing userdata
> Replace **$** with the number of the **userdata** partition, which should be **30**
> 
> If you have a **grow** partition, remove it as well
```cmd
rm $
```

#### Recreating userdata
> Replace **17.7GB** with the former start value of **userdata** which we just deleted
>
> Replace **60GB** with the end value you want **userdata** to have
```cmd
mkpart userdata ext4 17.7GB 60GB
```

#### Creating ESP partition
> Replace **60GB** with the end value of **userdata**
>
> Replace **60.3GB** with the value you used before, adding **0.3GB** to it
```cmd
mkpart esp fat32 60GB 60.3GB
```

#### Creating Windows partition
> Replace **60.3GB** with the end value of **esp**
>
> Replace **126GB** with the end value of your disk, use `p free` to find it
```cmd
mkpart win ntfs 60.3GB 126GB
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

### Reboot your phone
> Once it is booted, it will tell you decryption was unsuccesful and it will ask you to erase all data.
- Press this button to erase all data, let the phone boot back up, then reboot back to fastboot mode.

## [Next step: Installing Windows](2-install.md)












