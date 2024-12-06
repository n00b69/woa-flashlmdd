<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Expanding your phone's storage
> Due to LG bullshit, this phone only comes with a 128GB option and forced A/B partitions, which take up precious space. This guide will explain to you how you can delete the B partitions of your phone, to get back 8-10GB of extra space.

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Qfil](https://github.com/n00b69/woa-flashlmdd/releases/tag/Qfil)

- [QFILHelper](https://github.com/Beliathal/QFILHelper) Optional, for easier partition backups

- [Engineering ABL](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/engabl_ab.bin)
  
- [Modded TWRP](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/modded-twrp-v50.img)

### Notes
> [!WARNING]
>
> You will lose all of your data, so make sure you make a backup of your files beforehand. This guide does not instruct you on how to do this, because it will be different depending on the ROM you are using.
> 
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!
>
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/woahelperchat).

### Setting up Qfil
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
- If you see a **Download Fail:Sahara Fail** error, make sure your cable stays connected and reboot to EDL again by holding **volume down** + **power**, then start rapidly pressing the **volume up** button once the LG logo appears.
- Once you're back in EDL, try opening the Partition manager again.
- If it still fails, try to repeat the last step a few times. You can also try rebooting your phone and PC.

#### Backing up your partitions
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **vendor_a** > **Manage Partition Data** and press **Read Data**.
- Do the same thing for **abl_a**, **abl_b**, **system_a**, **product_a** and **OP_a**.
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
- Hold **volume down** + **power** until it shows the LG logo, then release the buttons.
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

#### Unmount data
> Ignore any possible errors and continue
>
> Replug the cable if it says "no devices/emulators found"
```cmd
adb shell umount /dev/block/by-name/userdata
```

### Preparing for partitioning
```cmd
adb shell parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions. Do not close the window, because you will need some values from this list as they may differ from the values used in this guide.
```cmd
print
```

#### Removing system partitions
> Replace **$** with the number of the **vendor_b** partition, which should be **23**
>
> Do the same thing for **system_a**, **system_b**, **product_a**, **product_b**, **OP_a** and **OP_b**
```cmd
rm $
```

#### Creating system_a partition
> Scroll back to the list that appeared when you ran `print`. Next to the names of the now removed partitions you will see three values. **Start**, **End**, and **Size**. **Size** will be important here.
> 
> Replace **2159MB** with the **End** value of **vendor_a**
>
> Use a calculator to add the **Size** of **system_a** to the **End** value, then replace **6181MB** with this value
```cmd
mkpart system_a ext2 2159MB 6181MB
```

#### Checking the partition size
> Sometimes parted will round down partition sizes. Make sure **system_a** actually ends at **6181MB**, or the value you used, by running the `print` command.
>
> If the **End** value is lower, for example **6180MB**, remove the partition and remake it, but this time add **1MB** to the end value (for example **6182MB**).

#### Creating the product_a partition
> Do the same thing with **product_a**, use a calculator to double check the values and use `print` to make sure the value is correct afterwards.
```cmd
mkpart product_a ext2 6181MB 8329MB
```

#### Creating the OP_a partition
> Do the same thing with **OP_a**, use a calculator to double check the values and use `print` to make sure the value is correct afterwards.
```cmd
mkpart OP_a ext4 8329MB 9063MB
```

#### Creating the userdata partition
> Replace **9063MB** with the end value of **OP_a**.
>
> ONLY if you have Windows installed, replace **126GB** with the start value of the **win** partition.
```cmd
mkpart userdata ext4 9063MB 126GB
```

#### Exit parted
```cmd
quit
```

### Reboot back into EDL
> To restore the contents of the partitions, or your phone will not boot

#### Restoring your partitions
- In **Qfil**, select Tools > Partition Manager, and click **Ok**.
- Right click on **system_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the backup you made earlier.
- Do the same thing for **product_a** and **OP_a**.

#### Erasing userdata
> [!Warning]
> DO NOT PRESS **Erase Entire Flash**, this will wipe your entire UFS!
- Select the **userdata** partition.
- Press **Erase** to erase its contents.
- Exit Partition Manager.

#### Reboot your device
> After exiting the partition manager, Qfil will reboot your phone to EDL. After this has happened, force reboot your phone by holding **volume down** + **power** until the LG logo appears.
>
> It may take a while before your device boots. If it doesn't, you may need to reflash your ROM with fastboot or EDL.

#### Disabling updates
> [!Important]
> After you've booted into Android, make sure you disable all system updates. These may softbrick your device by attempting to install updates to partitions that no longer exist.

## Finished!





