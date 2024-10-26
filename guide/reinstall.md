<img align="right" src="https://github.com/n00b69/woa-flashlmdd/blob/main/flashlmdd.png" width="350" alt="Windows 11 running on flashlmdd">

# Running Windows on the LG V50

## Reinstalling Windows

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- Any custom recovery

#### Boot into any custom recovery
> Make sure it supports ADB commands

### Formatting Windows and ESP partitions
```cmd
adb shell mkfs.ntfs -f /dev/block/by-name/win -L WINFLASH
```

```cmd
adb shell mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPFLASH
```

## [Next step: Reinstalling Windows](3-install.md)



















