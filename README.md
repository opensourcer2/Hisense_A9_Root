# How to root the Hisense A9
This instruction is adapted from this reddit [post](https://www.reddit.com/r/eink/comments/16tpr96/guide_how_to_root_the_hisense_a9/). This guide was originally written on XDA [here](https://forum.xda-developers.com/t/hisense-a9-root-reward-offered-snapdragon-662.4495809/page-11#post-88983317). 

#### ⚠️ Note: This process only works on Windows or Linux x64.

## Full Guide

### Prepare Phone

1. Navigate to **Settings > About Phone**, take note of the *Software* version.

1. Navigate to **Settings > About Phone**, then tap *Kernel* version several times to enable *Developer Options*.

1. Navigate to **Settings > System & Phone > Developer Options**

   - Enable `OEM unlocking`

   - Enable `USB debugging`

### Prepare Dependencies

1. Download the Android Platform tools. We need ADB from this software package. Select the appropriate version for your platform.

1. Download Magisk from their GitHub page.

1. Download the A9 Resources on Google Drive here. You will need:

   - A9 Fastboot by Denzil Ferreira

   - Factory-boot.img (if you have version L2037.6.04.06.00)

   - Latest-boot.img (if you have version L2037.6.08.01.00)

   - If you don’t have either, then follow the instructions on this post on how to retrieve your ROM’s boot.img.

1. Optional: download the Magisk version of LiteGapps on SourceForge. Choose from the folders depending on how many Google Apps you wish to install. Make sure to select the [MAGISK] version. Be sure to ignore the big green “Download Latest Version” button, this is a lie. I would personally suggest this one.

### Prepare Boot.img

1. Transfer the following files to your phone:

   - Magisk.apk

   - Boot.img (make sure you’re using the correct version for your phone)

1. Install Magisk, then follow prompts to select and prepare your Boot.img.

1. Transfer your newly prepared boot.img to your computer.

### Unlock and root your phone

1. Open two terminal windows.

1. Reference the fastboot folder using the cd command.(ex. cd ~/Downloads/A9_Fastboot/linux-x86/bin/)

   - To unlock your phone, you must use this version of Fastboot, or build your own to include the Hisense unlock command.

1. In your second terminal window, reference the platform-tools folder using the cd command.(ex. cd ~/Downloads/platform-tools/)

1. In the window pointing to the folder which contains adb, enter the following command:

   - `./adb reboot bootloader`

   - Once your phone reboots, you should see a screen that says “fastboot mode”

1. In your window pointing to your folder which contains fastboot, enter the following commands:

   - `./fastboot Hisense unlock`

   - `./fastboot erase avb_custom_key`

   - `./fastboot flash boot.img`

   - `./fastboot flash vbmeta --disable-verity --disable-verification vbmeta.img`

   - `./fastboot continue`

1. Your A9 should then prompt you to wipe all data. Click the button on the screen, then the upper volume rocker to accept.

1. Once your phone reboots, complete the setup.

   - If you get an error such as the launcher crashing or WiFi not working, you installed the wrong boot.img.

### Final touches

1. Move the following files onto your phone:

   - [MAGISK]-gapps-bla-bla.zip

1. Fix Magisk if it's not installing. See the FAQ for details on how to do this.

1. Load up your Gapps in Magisk.

   - Select install module.

   - Reboot.

1. Download Trichrome Library from here. : This prevents webview and Play Store hangups and crashes.

   - Install using adb: `./adb install trichrome.apk`

### Recommended Software

- Koreader -- arguably the best eInk optimized reading software on the planet.

- Calibre Sync -- for effortlessly syncing your Calibre eBook library.

- App Ops and Shizuku -- for properly managing permissions of system apps. You can use this to fix Google Contact sync, if it's giving you problems.

There you have it, you now have a superb, rooted eInk phone with all the comforts of home! If you need help beyond the context of this guide, I'd highly recommend visiting the original thread here. Now that the phone has been rooted, people have been doing all kinds of super cool things which improve an already great product.

## FAQ

Q: My OS version number is neither L2037.6.08.01.00 or L2037.6.04.06.00. Which boot.img do I use?

A: Neither. Using an incompatible boot.img causes you to lose WiFi. I would suggest updating your A9 to the latest version (L2037.6.08.01.00) by navigating to Settings > System & Updates > System updates. If you're upgrading from the earliest firmware, then you have to do a number of incrimental updates to get to the latest version. There is no combo update. Once you're there, then you can use the latest boot.img.

Q: I successfully installed the modified boot.img, but Magisk isn't detecting the install. How do I fix this?

A: This is a common problem. Uninstall Magisk and reboot your phone. A special installation app will appear in its place. Use this to reinstall Magisk, which will then prompt you to reboot a final time.

Q: Does rooting fix the problem of multi-tasking when I use a third party launcher?

A: No, sadly. App switching is built into the A9's launcher, though people have been exploring workarounds on XDA.

Q: Do I need an EDL cable?

A: No. This method is tested and working with the included USB-C cable that came with your A9.

Q: Wait, what's an EDL cable?

A: An EDL cable is designed for all Qualcomm phones to put them into Deep Flash Mode, also called Qualcomm 9008 Mode. You only need this cable if you've bricked your phone. Speaking of which...

Q: Things have gone terribly wrong. How do I get back to stock after bricking my phone?

A: Alas, you now need an EDL cable. This is because Hisense doesn't provide conventional .zip files with their full ROM's. This means that we can't use more conventional flashing methods, leaving us with just one option: Qualcomm's EDL mode.

Before we begin, there are a number of dependencies you need to acquire. EDL tools exist for Windows and Linux (like QDL for Linux), but the instructions here will assume you have Windows.

If you do have Linux, check out the XDA thread, as one of the members was able to figure it out. If you have MacOS, you need a virtual machine. While it's not free, I have been using Parallels with Windows 11, and I had it up and running in under half an hour.

Here's what you need to do:

    Download and install the following software, where applicable:

        The Hisense A9 firmware, which you can find here.

        QFIL 2.0.3.5

        Qualcomm HS-USB QDLoader 9008 Driver

    Reboot your computer.

    Unzip the RAR files. This is achieved simply by clicking the first one and then the software will figure out the rest.

    Open QFIL and select the following options:

        Select Build Type - Flat Build

        Programmer Path: Browse and select the prog_firehose_ddr_001360E1.elf file from the extracted firmware file.

        Load XML: Select rawprogram0_001360E1 to rawprogram5_001360E1. You don't need to select rawprogram_unspare0. Select patch0 to patch5.

        Storage Type: UFS (located on the bottom right side)

    Put your phone into EDL mode. This is achieved by having it reboot with the EDL cable unplugged. Then plug in your cable, and hold the power button, volume up, volume down, and the button on your EDL cable. Wait until the backlight turns off, then count down from ten. If your screen freezes with the backlight off, congratulations, you are now in EDL mode.

    You should now see your phone as an available port on QFIL. Click download.

        If it hangs and fails, put your phone into EDL again, or try using a standard USB-C cable for the download process.

        If it succeeds and your phone starts boot looping into a screen which says "fastboot mode", follow Step 5 and 6 again. Pushing the firmware twice generally fixes the bootloop for some reason.

    Turn your phone back on by holding the power button. You should now be directed to the setup screen.

Q: I've tried every single APN configuration known to humankind and my phone still isn't connecting to LTE.

A: Like the United States, Chinese domestic market phones use bands which are very specific to their region. Here there is good news and bad news. On one hand, carriers like T-Mobile use TD-LTE B41, which the A9 supports. Unfortunately, TD-LTE B41 is a fairly rare band, and is mostly found in densely populated urban areas.

If you are up to the challenge, you can try to unlock more bands by modifying your EFS, or encrypted file system. I've played with this a little, but it's dangerous territory. The Hisense A9 does not repair its EFS on wipe, and a lot of the guides available online are out of date. I've already lost my IMEI on one phone, and haven't been able to restore it.

You should also backup your EFS, but this is really hard without a recovery environment like TWRP and I haven't figured out a way to do it yet.

That being said, if you need your LTE bands to work at any cost, I would suggest visiting this XDA forum post and using this rather excellent LTE band calculator app to add support for the LTE bands you need. If you figure out how to get it working, let everyone know -- it would be a big deal for many A9 users!

Some final marginalia -- if you can no longer edit your NV items, you have not lost connectivity forever. Simply try restoring the QFIL backup you (hopefully) made earlier. If all else fails, you can perform a hard reset by doing the following:

    `./adb reboot fastboot`

    `./fastboot wipe modemst1`

    `./fastboot wipe modemst2`

This will reset your bad values and you will be able to reconnect to WiFi, if not cellular.

Good luck! 😃
