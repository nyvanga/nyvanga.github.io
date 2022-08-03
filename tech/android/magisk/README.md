# Magisk

[The Magic Mask for Android (Magisk)](https://github.com/topjohnwu/Magisk)

Magisk is a suite of open source software for customizing Android, supporting devices higher than Android 6.0.

Some highlight features:

- MagiskSU: Provide root access for applications
- Magisk Modules: Modify read-only partitions by installing modules
- MagiskBoot: The most complete tool for unpacking and repacking Android boot images
- Zygisk: Run code in every Android applications' processes

## Install with TWRP already installed

- Official [Patching Images](https://topjohnwu.github.io/Magisk/install.html#patching-images) instructions.

1. Download the [Magisk app](https://github.com/topjohnwu/Magisk/releases/latest) apk and install on device
1. Download the selected TWRP recovery image to the device
1. Open the Magisk app
1. Under "Magisk" click "Install"
1. Click "Next"
1. Click "Select and Patch a File"
1. Choose the TWRP recovery img file from above
1. Click "Let's go"
1. Done when "All done!" is shown

A file called "magisk_patched-....img" is now pressent, which can be flashed using [heimdall](../heimdall/).

Find a way to get the .img file onto the computer running heimdall.

Optional: Click the save icon in the top right corner to save the log. It contains more information than shown on screen.
