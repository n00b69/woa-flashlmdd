<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Dualbooting Android and Windows seamlessly

### Prerequisites
- [UEFI image](https://github.com/n00b69/woa-betalm/releases/tag/UEFI)
  
- [WOA Helper app](https://github.com/Marius586/WoA-Helper-update/releases/tag/WOA)

## Setting up the dualboot app
> This guide assumes you are rooted. If you aren't, please do that first using [this guide](root.md)

### Setup - Android
- Download and install the **WOA Helper** app, then open it and grant it root access.
- Download the **UEFI image** and place it inside the folder named `UEFI` in your internal storage.
- Close and repen the WOA Helper app and press the **QUICKBOOT TO WINDOWS** button.

#### Booting to Android
- Run the new **Android** shortcut on your desktop (you can also pin it to your start menu / taskbar for ease of access)

#### Booting to Windows
- Press **QUICKBOOT TO WINDOWS** inside the app, or use the newly created toggle in your quick settings panel

> [!Important]
> If you ever update or change your Android ROM, make sure to create a new **boot.img** backup (after rooting your phone!) and place it inside the **C:\ folder** in Windows, overwriting the old file.
>
> You can use the **BACK UP BOOT IMAGE** feature in the WOA Helper app to do so.

## Finished!









