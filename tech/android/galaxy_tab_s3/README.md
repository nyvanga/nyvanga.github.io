# Samsung Galaxy Tab S3

## Lineage OS

As of 2024.02.24:
- [TWRP for Galaxy Tab S3](https://xdaforums.com/t/recovery-unofficial-sm-t820-sm-t825-twrp-3-5-2_9-0-for-galaxy-tab-s3.4303163/)
   - [TWRP 3.5.2 is highly recommended](https://xdaforums.com/t/recovery-unofficial-sm-t820-sm-t825-twrp-3-5-2_9-0-for-galaxy-tab-s3.4303163/#post-85300101)
   - Download: [Galaxy Tab S3 WiFi (gts3lwifi, SM-T820)](https://drive.google.com/file/d/1TcVhQyH-CQLf_7jZmcx0DE74-93hKwDL/view?usp=drive_link)
   - [All versions](https://drive.google.com/drive/folders/1GQa4xx3dZvVxNV9Tyrmi8SJKrEAl41JX?usp=drive_link)
   - [Flash using heimdall](../heimdall/)
- [LineageOS](../lineageos/)
- [LineageOS 19.1 for Galaxy Tab S3](https://xdaforums.com/t/rom-unofficial-12l-eas-ota-sm-t820-sm-t825-2023-11-15-lineageos-19-1-for-galaxy-tab-s3.4418855/)
   - Download: [Galaxy Tab S3 WiFi (gts3lwifi, SM-T820)](https://drive.google.com/file/d/18GCe1WKwnk8N4au-Sw-XV7CSyph56NDI/view?usp=drive_link)
   - [HideNavBar Magisk module](https://github.com/Magisk-Modules-Alt-Repo/HideNavBar) ([Magisk](../magisk/))
- [LineageOS 20.0 for Galaxy Tab S3](https://xdaforums.com/t/rom-unofficial-13-eas-ota-sm-t820-sm-t825-2023-11-16-lineageos-20-0-for-galaxy-tab-s3.4510785/) (no ecryption!!)
   - Download: [Galaxy Tab S3 WiFi (gts3lwifi, SM-T820)](https://drive.google.com/file/d/1EpZNN_FVZtT2yz1chc2drYHcE8g8JIn9/view?usp=drive_link)

As of 2024.02.25:

Lineage OS 19.1 with Magisk and HideNavBar and encryption:

1. Wipe and format data from TWRP
1. Install Lineage OS 19.1 (and nothing else)
1. Boot tablet and do minimal setup
1. Reboot to recovery
1. Install Magisk from .zip (just rename .apk, but check that it is still supported)
1. Boot tablet
1. Open Magisk (should be present, with a anonymous icon)
1. Magisk will ask to finish setup. Do that and reboot.
1. Open Magisk again and check that it is fully installed.
1. Install HideNavBar, and choose options to make the pill bar as small as possible (it is a big screen)
1. It will ask to reboot. Do that.
1. Check that gestures and Nav bar is as expected.
1. Remove all icons from Nav Bar, otherwise it will stay on-top of app's.
1. Go to Settings and choose Encrypt under Security.
1. It will reboot, and hopefully not go into a boot-loop.

Done! All is working now.

Upgrading Lineage:

NO CLUE! Probably a full wipe and start over...

Rebooting into TWRP will prompt for password of encrypted /data, and I have not succeeded in mounting it.

So be carefull about starting automatic upgrades of OS or other stuff that needs the recovery to work.

## Debloat

Google/Android:
```
pm uninstall --user 0 com.android.documentsui
pm uninstall --user 0 com.android.dreams.basic
pm uninstall --user 0 com.android.dreams.phototable
pm uninstall --user 0 com.android.providers.partnerbookmarks
pm uninstall --user 0 com.google.android.apps.meetings
pm uninstall --user 0 com.google.android.apps.tachyon
pm uninstall --user 0 com.google.android.googlequicksearchbox
pm uninstall --user 0 com.google.android.music
pm uninstall --user 0 com.google.android.play.games
pm uninstall --user 0 com.google.android.talk
pm uninstall --user 0 com.google.android.videos
pm uninstall --user 0 com.google.ar.lens
```

Samsung:
```
pm uninstall --user 0 com.samsung.aasaservice
pm uninstall --user 0 com.samsung.accessibility
pm uninstall --user 0 com.samsung.android.allshare.service.fileshare
pm uninstall --user 0 com.samsung.android.allshare.service.mediashare
pm uninstall --user 0 com.samsung.android.calendar
pm uninstall --user 0 com.samsung.android.clipboarduiservice
pm uninstall --user 0 com.samsung.android.contacts
pm uninstall --user 0 com.samsung.android.email.provider
pm uninstall --user 0 com.samsung.android.galaxycontinuity
pm uninstall --user 0 com.samsung.android.game.gos
pm uninstall --user 0 com.samsung.android.voc
pm uninstall --user 0 com.samsung.android.app.assistantmenu
pm uninstall --user 0 com.samsung.android.app.clockpack
pm uninstall --user 0 com.samsung.android.app.notes
pm uninstall --user 0 com.samsung.android.app.readingglass
pm uninstall --user 0 com.samsung.android.app.reminder
pm uninstall --user 0 com.samsung.android.app.smartcapture
pm uninstall --user 0 com.samsung.android.app.social
pm uninstall --user 0 com.samsung.android.app.soundpicker
pm uninstall --user 0 com.samsung.android.app.talkback
pm uninstall --user 0 com.samsung.android.calendar
pm uninstall --user 0 com.samsung.android.contacts
pm uninstall --user 0 com.samsung.android.dialer
pm uninstall --user 0 com.samsung.android.dsms
pm uninstall --user 0 com.samsung.android.fmm
pm uninstall --user 0 com.samsung.android.messaging
pm uninstall --user 0 com.samsung.android.weather
pm uninstall --user 0 com.samsung.android.wellbeing
pm uninstall --user 0 com.samsung.app.newtrim
pm uninstall --user 0 com.samsung.faceservice
pm uninstall --user 0 com.skype.raider
```

Samsung Experience Service (SEC):
```
pm uninstall --user 0 com.sec.android.app.chromecustomizations
pm uninstall --user 0 com.sec.android.app.clockpackage
pm uninstall --user 0 com.sec.android.app.magnifier
pm uninstall --user 0 com.sec.android.app.myfiles
pm uninstall --user 0 com.sec.android.app.popupcalculator
pm uninstall --user 0 com.sec.android.app.sbrowser
pm uninstall --user 0 com.sec.android.widgetapp.samsungapps
pm uninstall --user 0 com.sec.android.widgetapp.webmanual
pm uninstall --user 0 com.sec.penup
pm uninstall --user 0 com.sec.spp.push
```

Samsung kids:
```
pm uninstall --user 0 com.sec.android.app.kidshome
pm uninstall --user 0 com.sec.kidsplat.camera
pm uninstall --user 0 com.sec.kidsplat.drawing
pm uninstall --user 0 com.sec.kidsplat.kidsgallery
pm uninstall --user 0 com.sec.kidsplat.kidstalk
pm uninstall --user 0 com.sec.kidsplat.media.kidsmusic
```

Microsoft:
```
pm uninstall --user 0 com.microsoft.office.excel
pm uninstall --user 0 com.microsoft.office.onenote
pm uninstall --user 0 com.microsoft.office.powerpoint
pm uninstall --user 0 com.microsoft.office.word
```
