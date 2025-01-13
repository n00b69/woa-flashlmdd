<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Partitioning your device

### Prerequisites
- Unlocked bootloader

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Qfil](https://github.com/n00b69/woa-flashlmdd/releases/tag/Qfil)

- [QFILHelper](https://github.com/Beliathal/QFILHelper) Optional, for easier partition backups

- [Engineering ABL](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/engabl_ab.bin)
  
- [Modded TWRP](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/modded-twrp-v50.img)

### Notes
> [!WARNING]  
>  
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!
>
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/woahelperchat).

### Opening CMD as an admin
> Download **platform-tools** and extract the folder somewhere, then open CMD as an **administrator**.
>
> It is recommended to keep this window open and use it throughout the entire guide.
> 
> Replace `path\to\platform-tools` with the actual path to the platform-tools folder, for example **C:\platform-tools**.
```cmd
cd path\to\platform-tools
```

### Backing up partitions
> If you don't do this and mess something up, you're on your own

#### Setting up Qfil
- Open **Qfil**.
- In "Select Build Type", select **flat build**.
- In "Select programmer", select the downloaded firehose.
- In "Configuration", make sure the "Device Type" is set to **UFS**.

#### Boot into EDL
- Open **Device Manager** on your PC
- With the phone turned off, hold **volume down** + **power**.
- After the LG logo appears, while still holding **volume down** + **power**, start rapidly pressing the **volume up** button.
- Keep doing this until you hear a USB connection sound on your PC, or when **Qualcomm HS-USB QDLoader 9008** appears in the **Ports (COM & LPT)** category of Device Manager.
> [!Note]
> If the device is called **QUSB_BULK_CID** or has a ⚠️ yellow warning triangle / question mark, and is located in any other category (for example **Other devices**), you need to install EDL drivers first.
- To install EDL drivers, extract the contents of **QUD.zip** somewhere, right click on **QUSB_BULK_CID**, click on **Update driver** and **Browse my computer for drivers**, then find and select the **QUD** folder.

#### Making sure Qfil works
- In **Qfil**, make sure the correct port is selected. If it says `No Port Available`, select the **Qualcomm HS-USB QDLoader 9008** port.
- At the top, select "Tools" > "Partition manager", and click **Ok**.
> [!Note]
> If you see a **Download Fail:Sahara Fail** or **Download Fail:FireHose Fail:FHLoader Fail:Process Fail** error, make sure your cable stays connected and reboot to EDL again by holding **volume down** + **power**.
- Once you're back in EDL, try opening the Partition manager again.
- If it still fails, try to repeat the last step a few times. You can also try rebooting your phone and PC.

#### Backing up your partitions
- In the Partition manager, right click on **abl_a** > **Manage Partition Data** and press **Read Data**.
- Do the same thing for **fsg**, **fsc**, **modemst1**, **modemst2** and **modem_a**
- If you want to be on the safe side, you can also use [QFILHelper](https://github.com/Beliathal/QFILHelper) to additionally back up every partition. In this guide we only back up the most critical partitions.

> [!Important]
> Navigate to `C:\Users\YOURNAME\AppData\Roaming\Qualcomm\QFIL\COMPORT_#\` and rename the backed up partitions one by one as you back them up. Qfil does not name the backups, and if you don't rename them, it'll be impossible to figure out which files are which. You can restore them later with the **Load Image** function.

### Flashing engineering ABL
> If fastboot works on your phone, you can skip this step
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **abl_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **engabl_ab.bin** file.
- Do the same thing for **abl_b**.

#### Reboot into fastboot mode
- Reboot your phone by holding **volume down** + **power** until it shows the LG logo, then release the buttons.
- After it has booted, unplug the cable and power it off.
- Once the device has turned off, hold the **volume down** button, then plug the cable back in.
> [!Note]
> If the phone in device manager is called **Android** and has a ⚠️ yellow warning triangle, you need to install fastboot drivers before you can continue.
- To install fastboot drivers, extract the contents of **QUD.zip** somewhere, right click on **Android**, click on **Update driver** and **Browse my computer for drivers**, then find and select the **QUD** folder.

#### Boot into the modded TWRP
> Replace `path\to\modded-twrp-v50.img` with the actual path of the provided TWRP image
>
> After booting into TWRP, leave the device on the main screen. You can press the power button to turn the display off, if you want
```cmd
fastboot boot path\to\modded-twrp-v50.img
```

### Backing up your boot image
> This will back up your boot image in the current directory (which should be the **platform-tools** folder)
>
> Replug the cable if it says "no devices/emulators found"
```cmd
adb shell "dd if=/dev/block/platform/soc/1d84000.ufshc/by-name/boot$(getprop ro.boot.slot_suffix) of=/tmp/boot.img" && adb pull /tmp/boot.img
```

#### Unmount data
> Ignore any possible errors and continue
```cmd
adb shell umount /dev/block/by-name/userdata
```

### Preparing for partitioning
```cmd
adb shell parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, **userdata** or **grow** should be the last partition in the list
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
> Replace **64GB** with the end value you want **userdata** to have. In this example Android will have 64GB-17.7GB = **46.3GB** of usable storage space.
```cmd
mkpart userdata ext4 17.7GB 64GB
```

#### Creating ESP partition
> Replace **64GB** with the end value of **userdata**
>
> Replace **64.3GB** with the value you used before, adding **0.3GB** to it
```cmd
mkpart esp fat32 64GB 64.3GB
```

#### Creating Windows partition
> Replace **64.3GB** with the end value of **esp**
```cmd
mkpart win ntfs 64.3GB 126GB
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

### Format all data
- Go to the Wipe menu in your recovery and wipe all data. If this doesn't work, simply reboot your phone.

#### Reboot your phone
> Once it is booted, it might tell you decryption was unsuccesful and it will ask you to erase all data.
- Press this button to erase all data, then set up your phone (make sure to also enable USB debugging in developer settings), then reboot back into TWRP.

### Formatting win and esp partitions
> After rebooting back into TWRP
```cmd
adb shell mkfs.ntfs -f /dev/block/by-name/win -L WINFLASH
```
```cmd
adb shell mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPFLASH
```

### Fixing the GPT
> If you do not do this, Windows may break your device
```cmd
adb shell fixgpt
```

#### Reboot your phone
> In preparation for the next step

## [Next step: Rooting your phone](2-root.md)












