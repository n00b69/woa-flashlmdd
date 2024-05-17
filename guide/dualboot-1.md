<img align="right" src="https://github.com/n00b69/woa-betalm/blob/main/betalm.png" width="350" alt="Windows 11 running on betalm">

# Running Windows on the LG G8s

## Dualbooting Android and Windows seamlessly

### Prerequisites
- [UEFI image](https://github.com/n00b69/woa-betalm/releases/tag/UEFI)
  
- [WOA Helper app](https://github.com/Marius586/WoA-Helper-update/releases/tag/WOA)
  
- [Switch To Android package](https://github.com/n00b69/woa-betalm/releases/download/Files/betalm-sta.zip)

## Setting up the dualboot app
> This guide assumes you are rooted, if you aren't, please do that first

### Setup - Android
- Download and install the WOA Helper app, then open it and grant it root access.
- Download the UEFI image and place it inside the folder named `UEFI` in your internal storage.
- Press the `Mount Windows` button to mount Windows to your internal storage at `/sdcard/Windows`
> [!Important]
> If `/sdcard/Windows` is empty, your rom does not support mounting and you will have to make a boot.img backup inside the app, then copy it manually to Windows once you boot to it (for example by uploading it somewhere and then downloading it while booted into Windows). The same applies to the STA files.
>
> Do the same thing if the folder is read-only.
- Create a folder called `sta` in Windows and unpack the four files in the `Switch to Android package` file here (the files should go to `/sdcard/Windows/sta`
- Return to the WOA Helper app and press the `Quickboot` button.

### Setup - Windows
- Navigate to C:\sta and create a shortcut of `sta.exe` to your desktop.

#### Booting to Android
- Run the new shortcut on your desktop (you can also pin it to your start menu / taskbar for ease of access)

#### Booting to Windows
- Press `Quickboot to Windows` inside the app, or use the newly created toggle in your quick settings panel

## Finished!

