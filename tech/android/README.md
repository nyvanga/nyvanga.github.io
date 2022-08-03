# Android ADB

Firstly enable USB debugging on the phone.

1. Go into `About phone`
1. Go into `Software information`.
1. Click multiple times on `Build-number`.
1. It will ask for pin or other authentication.
1. The message `Developer mode has been enabled` should show up.
1. Go back to the main settings menu.
1. At the bottom, below `About phone`, go into `Developer options`.
1. Scroll down and enable `USB debugging`.

Connect with usb cable to computer with `ADB` installed, and run:
```
adb devices
```
Output:
```
* daemon not running; starting now at tcp:5037
* daemon started successfully
List of devices attached
***********	unauthorized
```

Choose to allow computer on the phone, and maybe remember computer if desired.

Run adb devices again, and the output should look like this:
```
List of devices attached
*********** device
```
# Extract APK file

Find the package:
```
adb shell pm list packages
```
Output:
```
package:com.android.qns
package:clint.asgard
package:dk.ok.android.findtank

and many more ...
```

Locate the APK file:
```
adb shell pm path clint.asgard
```
Output:
```
package:/data/app/~~T5PAVV7DAmK5dnT-baaEWg==/clint.asgard-_v1jHV3RkfNo92W5oBmxQA==/base.apk
```

Pull the APK file:
```
adb pull /data/app/~~T5PAVV7DAmK5dnT-baaEWg==/clint.asgard-_v1jHV3RkfNo92W5oBmxQA==/base.apk ~/Downloads/clint_asgard.apk
```
Output:
```
/data/app/~~T5PAVV7DAmK5dnT-baaEWg==/clint.asgard-_v1jHV3RkfNo92W5oBmxQA==/base.apk: 1 file pulled, 0 skipped. 34.7 MB/s (7072661 bytes in 0.194s)
```

# Custom recovery on Samsung devices

- [Flash recovery TWRP on Samsung devices using heimdall](heimdall/)

# LineageOS

- [LineageOS](lineageos/)

# The Magic Mask for Android

- [Magisk](magisk/)

# Debloat using `adb shell`

[Source](https://youtu.be/X01hZJtfMJg)

Device specific debloating:
- [Nvidia Shield](nvidia_shield/)
- [Samsung Galaxy Tab S3](galaxy_tab_s3/)
- [Samsung Galaxy Tab A7](galaxy_tab_a7)
- [Samsung ZFold 3](z_fold3/)

Get into the shell by running:
```
adb shell
```
If the prompt doesn't show up, check steps above to allow computer to connect.

To list currently installed packages:
```
pm list packages
```

To disable but not uninstall (Useful if testing or unsure if you need something):
```
pm disable-user --user 0 <package id>
```

To list currently installed but disabled packages:
```
pm list packages -d
```

To re-enable:
```
pm enable --user 0 <package id>
```

Install command:
```
pm install-existing --user 0 <package id>
```

```
list packages [-f] [-d] [-e] [-s] [-3] [-i] [-l] [-u] [-U] [--uid UID] [--user USER_ID] [FILTER]

Prints all packages; optionally only those whose name contains
the text in FILTER.
Options:
  -f: see their associated file
  -d: filter to only show disabled packages
  -e: filter to only show enabled packages
  -s: filter to only show system packages
  -3: filter to only show third party packages
  -i: see the installer for the packages
  -l: ignored (used for compatibility with older releases)
  -U: also show the package UID
  -u: also include uninstalled packages
  --uid UID: filter to only show packages with the given UID
  --user USER_ID: only list packages belonging to the given user
```
