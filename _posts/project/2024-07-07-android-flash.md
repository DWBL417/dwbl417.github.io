---
layout: post
title: Android之Pixel 刷机
category: project
description: 07-07 | Android FLash
---

1. pixel 升级到android 13

2. 下载adb工具，OTA镜像和Factory image 镜像

3. 手机进入Recovery模式，通过adb工具刷入ota，确保side slot 有可启动的android 13 镜像，避免无法启动

4. 下载adb工具和OTA镜像，Factory image 镜像

5. 手机flash image需开启oem锁和USB debugging

6. USB 调试进入fastboot模式（bootloader），可选解锁bootloader，此时会擦除数据

7. 执行镜像安装脚本，刷机后关闭BL锁

8. 手机上的oem锁只是确认是否允许解锁，开oem锁才能下一步通过fastboot解锁bootloader

9. Bootloader锁就是限制用户刷第三方ROM和第三方Recovery以及限制Root的“锁”，能通过官方Recovery刷入官方签名的ROM包，不可降级，以及大部分手机不可以Root
需解了BootLoader替换Recovery才行。对于一般刷机来说，bootloader锁定可以大大减小死砖的几率。


##  [Factory Images for Nexus and Pixel Devices](https://developers.google.com/android/images)

You will find these files useful if you have flashed custom builds on your device, and wish to return your device to its factory state.

Note that it's typically easier and safer to **sideload the full OTA image instead.**

If you do use a factory image, please make sure that you **re-lock your bootloader when the process is complete.**

### Updating Pixel 6, Pixel 6 Pro, and Pixel 6a devices to Android 13 for the first time

After taking an Android 13 update and successfully booting the device post update, an Android 12 build resides in the inactive slot (seamless updates for more information on slots) of the device. The inactive slot contains an older bootloader whose anti-rollback version has not been incremented. If the active slot is then flashed with a build that fails to boot, the fallback mechanism of seamless updates kicks in and the device tries to boot from the inactive slot. Since the inactive slot contains the older bootloader, the device enters an unbootable state.

To avoid hitting this state, if you are flashing a Pixel 6, Pixel 6a, or Pixel 6 Pro device with an Android 13 build for the first time, please flash the bootloader partition to the inactive slot **after successfully updating and booting into Android 13 at least once.**

* Option 1 (recommended): After a successful boot into Android 13 for the first time, sideload the **full OTA image** corresponding to that build and reboot the device to ensure that **both slots have a bootable image.**

### [Full OTA Images for Nexus and Pixel Devices](https://developers.google.com/android/ota)

**This has the same effect as flashing the corresponding factory image, but without the need to wipe the device or unlock the bootloader.**

To update a device using one of the OTA images below, you need the latest **adb tool**. You can get it from the **[Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools)** package, which you can download here.

Don't forget to either **add adb to your PATH environment variable** or change into the directory containing the executable.

Also be certain that you've set up **USB access** for your device, as described in Run Apps on a Hardware Device.

To apply an OTA update image:

1. Make sure that there is no pending OTA update, by going to **Settings > About phone > System updates**, which should say "Your system is up to date".

2. Download the appropriate update image for your device below.

3. Verify the checksum of the image: the OTA mechanism has a built-in validation feature, but verifying will save you some time if the file is incomplete. The last portion of the filename is the first 8 digits of its SHA-256 checksum; the full SHA-256 checksum is also shown next to the download link.

4. With the device powered on and **USB debugging enabled**, 

execute:

	adb reboot recovery

If you're unable to use adb to reboot into recovery, you can use the key combination for your device instead, and then select the **Recovery**option from the bootloader menu. The device is now in recovery mode and an Android logo with red exclamation mark should appear on screen.

5. To access the recovery menu, hold the **Power** button and press **Volume Up** once. The recovery text menu will appear.

6. To enter sideload mode, select the option **Apply update from ADB**.

7. Run the following command:

and check that your device shows up with "sideload" next to its name.

	adb devices

8. Run the following command:

where ota_file.zip is the name of the file you have downloaded and verified.

    adb sideload ota_file.zip

9. Once the update finishes, reboot the phone by choosing Reboot system now.

For security, you should disable USB debugging when the device is not being updated.

### [Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools)

Android SDK Platform-Tools is a component for the Android SDK. It includes tools that interface with the Android platform, primarily adb and fastboot.This download is useful if you want to use **adb** directly from the command-line and don't have Studio installed.**fastboot** is needed if you want to unlock your device bootloader and flash it with a new system image.

### Flashing instructions

The factory image downloaded from this page includes a script that flashes the device, typically named **flash-all.sh** (On Windows systems, use flash-all.bat instead).

Warning: To flash a device using one of the system images below (or one of your own), you also need the latest fastboot tool. You can get it from the Android SDK Platform-Tools package, which you can download here.

Once you have the fastboot tool, add it to your **PATH environment variable** (the flash-all script below must be able to find it).

Also be certain that you've set up **USB access** for your device, as described in Run Apps on a Hardware Device.

Caution: Flashing a new system image deletes all user data. Be certain to first backup any personal data such as photos.

**To flash a system image:**

1. Download the appropriate system image for your device below, then unzip it to a safe directory.

2. Connect your device to your computer over USB.

3. In Settings, tap System, then tap Developer options and **enable OEM unlocking and USB debugging**.

Enable OEM unlocking and USB debugging in the developer options:

In the Settings app, tap **About phone**.

Tap **Build number** seven times.

When you see the message **You are now a developer!**, tap **<-**.

Tap **System**, then tap **Developer options**.

Enable **OEM unlocking and USB debugging**. 

If OEM unlocking is unavailable, connect to the internet so the device can check in. If that still doesn't work, you can force a check in: In the Dialer app, **enter *#*#CHECKIN#*#* (*#*#2432546#*#*) (no SIM required)**. After entering the number (no need to press call), the text disappears and a success notification appears.

If OEM unlocking remains unavailable, your device might be SIM locked by your carrier and the bootloader can't be unlocked.

4. Start the device in **fastboot mode** with one of the following methods:

Using the adb tool: With the device powered on, execute:

	adb reboot bootloader

Using a key combo: Turn the device off, then turn it on and immediately hold down the relevant key combination for your device.

5. If necessary, unlock the device's bootloader using one of the following methods:

If you are updating a Nexus or Pixel device that is manufactured in 2015 or later (for example, a Nexus 5X, Nexus 6P, Pixel, Pixel XL, Pixel 2 or Pixel 2 XL device), run this command:

    fastboot flashing unlock

Note: the 'flashing unlock' command is only available with fastboot version 23.0.1 or later. The latest available version of fastboot can be downloaded from SDK Platform Tools.

For Pixel 2: To flash the bootloader, Pixel 2's boot loader must be updated to at least Oreo MR1's version first. This may be done by applying an over-the-air (OTA) update, or sideloading a full OTA with the instructions on that page.

For Pixel 2 XL only with loader version prior to TMZ20a: the critical partitions may also need to be unlocked before flashing. The unlock can be performed with this command, and should NOT be done on other devices:

	fastboot flashing unlock_critical

If you are updating an older device, run this command:

    fastboot oem unlock

The target device will show you a confirmation screen. (This erases all data on the target device.)

See Unlocking the bootloader for more detailed instructions.

6. Open a terminal and navigate to the unzipped system image directory.

7. Execute the **flash-all script**. This script installs the necessary bootloader, baseband firmware(s), and operating system.

Tip: if you're seeing adb devices output before reboot but fastboot or the flash script are misbehaving, it might be issues with your USB cable. Try a different port and/or switching connectors. If you are using a USB C port on your computer try a USB A port instead.

Once the script finishes, your device reboots. You should now lock the bootloader for security:

Start the device in fastboot mode again, as described above.

Execute:

	fastboot flashing lock

or, for older devices, run:

    fastboot oem lock

Locking bootloader will wipe the data on some devices. After locking the bootloader, if you want to flash the device again, you must run fastboot oem unlock again, which will wipe the data.
