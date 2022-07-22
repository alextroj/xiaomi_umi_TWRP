# For building TWRP for Xiaomi Mi 10 / 10 Pro

TWRP device tree for Xiaomi Mi 10 and Mi 10 Pro

Kernel and all blobs are extracted from [miui_CMI_21.9.17_1c6ed0daa1_11.0.zip](https://hugeota.d.miui.com/21.9.17/miui_CMI_21.9.17_1c6ed0daa1_11.0.zip) firmware.

The Xiaomi Mi 10 (codenamed _"umi"_) and Xiaomi Mi 10 Pro (codenamed _"cmi"_) are high-end smartphones from Xiaomi.

Xiaomi Mi 10 / 10 Pro was announced and released in February 2020.

## Device specifications

| Device       | Xiaomi Mi 10 / 10 Pro                       |
| -----------: | :------------------------------------------ |
| SoC          | Qualcomm SM8250 Snapdragon 865              |
| CPU          | 8x Qualcomm® Kryo™ 585 up to 2.84GHz        |
| GPU          | Adreno 650                                  |
| Memory       | 8GB / 12GB RAM (LPDDR5)                     |
| Shipped Android version | 10                               |
| Storage      | 128GB / 256GB / 512GB UFS 3.0 flash storage |
| Battery      | Non-removable Li-Po 4780mAh                 |
| Dimensions   | 162.58 x 74.8 x 8.96 mm                     |
| Display      | 2340 x 1080 (19.5:9), 6.67 inch             |

## Device picture

![Xiaomi Mi 10](https://cdn.cnbj0.fds.api.mi-img.com/b2c-shopapi-pms/pms_1581494372.61732687.jpg)

## Features

Works:

- [X] ADB
- [X] Decryption of /data (MIUI - password/pattern only!!!)
- [X] Decryption of /data (AOSP - Roms with wrapped key support only!!!)
- [X] Screen brightness settings
- [X] Vibration support
- [X] MTP
- [X] Flashing (opengapps, roms, images and so on)
- [X] USB OTG
- [X] Fasbootd
- [X] Sideload (adb sideload update.zip)
- [X] Reboot to bootloader/recovery/system/fasbootd
- [X] F2FS/EXT4 Support, exFAT/NTFS where supported

## Compile

First checkout minimal twrp with aosp tree:

```
repo init -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-12.1
repo sync
```

Then add these projects to .repo/manifest.xml:

```xml
<project path="device/xiaomi/umi" name="alextroj/xiaomi_umi_TWRP" remote="github" revision="android-12.1" />
```

Then edit .repo/manifest/twrp-default.xml:

```xml
<project path="bootable/recovery" name="alextroj/android_bootable_recovery" remote="github" revision="android-12.1" />
```

Finally execute these:

```
. build/envsetup.sh
lunch twrp_umi-eng
mka recoveryimage ALLOW_MISSING_DEPENDENCIES=true # Only if you use minimal twrp tree.

```
## Special Notes for this branch
- Device makefile in the device tree and dependencies file should use the "twrp" prefix.
- Currently, decryption on 12.1 is a work in progress (WIP). Decryption is only fully functional (i.e. works with password/PIN/pattern) on legacy Pixel devices that use weaver but do not use wrappedkey. On other devices, decryption will only work if no PIN is set in Android.
- FDE decryption is not presently supported in this branch.
- In order to successfully build in this branch, the following patch(es) will need to be cherry-picked:

    - [fscrypt: wip](https://gerrit.twrp.me/c/android_bootable_recovery/+/5405)
    - [fscrypt: move functionality to libvold](https://gerrit.twrp.me/c/android_system_vold/+/5540)

## To test it:

```
fastboot boot out/target/product/umi/recovery.img
```

## Thanks
- [FsCrypt fix by mauronofrio](https://github.com/mauronofrio/android_bootable_recovery)
- [Decryption by bigbiff](https://github.com/bigbiff/android_bootable_recovery)
- [Oneplus 8 TWRP by mauronofrio](https://github.com/mauronofrio/android_device_oneplus_instantnoodle_TWRP)
