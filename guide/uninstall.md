<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Uninstalling Windows
> There are two methods to uninstall Windows; manually, or by restoring the stock rom. If you are using a custom rom and want to keep this rom, method 2 is recommended.

## Method 1: Restoring the stock ROM

### Prerequisites
- [Stock ROM (.kdz)](https://docs.google.com/spreadsheets/d/10G72--uM5viwszJt8eY0q5PUjC53yzS2jvncdiJPaEk)

- [Patched LG Up](https://drive.google.com/file/d/1JkUfZ26-zajXLc-NDQFrOWCbwKDQvIZ/view?usp=sharing)

### Entering Download Mode
- While your device is turned off, hold the **volume up** button, then plug in a USB cable that is connected to your PC.

> [!Note]
> If Download Mode does not start, you may need to restore the original **abl** file with QFil, which you should have backed up when partitioning your device (located at `C:\Users\YOURNAME\AppData\Roaming\Qualcomm\QFIL\COMPORT_#\`

### Flashing your stock ROM
- Open **LG Up** while your device is booted into **Download Mode**.
- Select the stock ROM .kdz & select the `Refurbish` option in **LG Up**, then start flashing.
- Wait for the process to finish and your device should automatically reboot.

## Finished!


## Method 2: Uninstalling Windows manually 
### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Modded TWRP](https://github.com/n00b69/woa-flashlmdd/releases/download/Files/modded-twrp-v50.img)

### Notes
> [!WARNING]
> 
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!
>
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/woahelperchat). 

### Reboot into fastboot mode
> If you don't have access to fastboot, use the instructions in the [partitioning guide](1-partition.md) to flash the engineering ABL.
- With the device turned off, hold the **volume down** button, then plug the cable in.

### Opening CMD as an admin
> Download **platform-tools** and extract the folder somewhere, then open CMD as an **administrator**.
>
> It is recommended to keep this window open and use it throughout the entire guide.
> 
> Replace `path\to\platform-tools` with the actual path to the platform-tools folder, for example **C:\platform-tools**.
```cmd
cd path\to\platform-tools
``` 

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

### Preparing parted
```cmd
adb shell parted /dev/block/sda
``` 

#### Delete Windows Partition
> Use `print all` to make sure that partition 32 is Windows
```cmd
rm 32
``` 

#### Delete ESP Partition
> Use `print all` to make sure that partition 31 is ESP
```cmd
rm 31
``` 

#### Resize userdata Partition
> Use `print all` to make sure that partition 30 is userdata
>
> These are two different commands, run them seperately
```cmd
resizepart 30
```
```cmd
126GB
``` 

#### Exit Parted
```cmd
quit
``` 

### Format all data
- Go to the Wipe menu in your recovery and wipe all data. If this doesn't work, simply reboot your phone.

#### Reboot your phone
> Once it is booted, it might tell you decryption was unsuccesful and it will ask you to erase all data.
- Press this button to erase all data.

## Finished!






















