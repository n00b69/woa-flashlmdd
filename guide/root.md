<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Rooting your device

### Prerequisites
- [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- Any custom recovery (easy method)

- [Qfil](https://github.com/n00b69/woa-flashlmdd/releases/tag/Qfil) (harder method)

## Method 1: Easy method
> If you don't have LineageOS installed, use method 2

#### Boot into Lineage recovery
> By holding the **volume up** + **power** buttons while the phone is powered off

### Flashing Magisk
- Select **Apply update** > **Apply from ADB**.
- Download **magisk.apk** and rename it to **magisk.zip**
- Use the adb sideload command to install Magisk. Replace `path\to\magisk.zip` with the actual path of magisk.zip.
```cmd
adb sideload path\to\magisk.zip
```
- Reboot back to Android after it has finished flashing.

#### Finishing the Magisk installation
- Find the Magisk app and open it. Let it update if it asks you to update.
- Once you are able to open Magisk, open it and let it finish the set up. It should reboot after a few seconds.

## Finished!

## Method 2: Harder method
> Use this method if the first one does not work

### Setting up Qfil
- Open **Qfil**.
- In "Select Build Type", select **flat build**.
- In "Select programmer", select the downloaded firehose.
- In "Configuration", make sure the "Device Type" is set to **UFS**.

#### Boot to EDL
- Open **Device Manager** on your PC
- With the phone turned off, hold **volume down** + **power**.
- After the LG logo appears, while still holding **volume down** + **power**, start rapidly pressing the **volume up** button.
- Keep doing this until you hear a USB connection sound on your PC, or when **Qualcomm HS-USB QDLoader 9008** appears in the **Ports (COM & LPT)** category of Device Manager.
- If the device is called **QUSB_BULK_CID** or has a ⚠️ yellow warning triangle / question mark, and is located in any other category (for example **Other devices**), you need to install EDL drivers first.
- To install EDL drivers, extract the contents of **QUD.zip** somewhere, right click on **QUSB_BULK_CID**, click on **Update driver** and **Browse my computer for drivers**, then find and select the **QUD** folder.

#### Making sure Qfil works
- In **Qfil**, make sure the correct port is selected. If it says `No Port Available`, select the **Qualcomm HS-USB QDLoader 9008** port.
- At the top, select "Tools" > "Partition manager", and click **Ok**.
- If you see a **Download Fail:Sahara Fail** error, make sure your cable stays connected and reboot to EDL again by holding **volume down** + **power**, then start rapidly pressing the **volume up** button once the LG logo appears.
- Once you're back in EDL, try opening the Partition manager again.
- If it still fails, try to repeat the last step a few times. You can also try rebooting your phone and PC.

#### Backing up your boot image
- In the Partition manager, right click on **boot_a** > **Manage Partition Data** and press **Read Data**.
- Navigate to `C:\Users\YOURNAME\AppData\Roaming\Qualcomm\QFIL\COMPORT_#\` and rename the backed up partition to **boot.img** (make sure it changes the extension).

#### Reboot back to Android
- Hold **volume down** + **power** until it shows the LG logo, then release the buttons.

### Patching the boot image
- Copy the **boot** backup to your phone.
- Download and install **Magisk**, then open it.
- Press **Install** > **Patch a file** and select the **boot** image.
- Once it's done, copy the patched boot image to your PC and reboot back into EDL.

#### Flashing your rooted boot image
- Return to Qfil and open the Partition manager.
- In the Partition manager, right click on **boot_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **patched boot.img** file.
- Do the same thing for **boot_b**.

#### Reboot back to Android
- Hold **volume down** + **power** until it shows the LG logo, then release the buttons.

## Finished!
