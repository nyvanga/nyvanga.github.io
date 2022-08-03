# Nvidia Shield

## Enable remote ADB

Enable developer mode:

1. Go into `Settings`
1. Go into `Device Preferences`
1. Go into `About`
1. Click 7 times on `Build`
1. The message `You are now a developer` should show up.

Enable network debugging

1. Goto Device Preferences -> Developer options
1. Enable Network debugging
1. Note the IP and port (takes a few seconds to show up)

On a computer with `ADB` installed, and run:
```
adb connect <ip from above>:<port from above>
```
Output:
```
* daemon not running; starting now at tcp:<some port>
* daemon started successfully
failed to authenticate to <ip from above>:<port from above>
```
On the Shield a dialog asking to allow asking is showed. Choose allow.

## Default to custom launcher

Trick is to disable the default Google garbage launcher:

```
pm uninstall --user 0 com.google.android.tvlauncher
pm uninstall --user 0 com.google.android.tvrecommendations
pm uninstall --user 0 com.google.android.leanbacklauncher
pm uninstall --user 0 com.google.android.leanbacklauncher.recommendations
```

## Disable bloatware

Look through the list. This is just what I like to remove:

```
pm uninstall --user 0 air.com.vudu.air.DownloaderTablet
pm uninstall --user 0 air.ITVMobilePlayer
pm uninstall --user 0 com.amazon.amazonvideo.livingroom
pm uninstall --user 0 com.amazon.amazonvideo.livingroom.nvidia
pm uninstall --user 0 com.amazon.music.tv
pm uninstall --user 0 com.android.dreams.basic
pm uninstall --user 0 com.android.gallery3d
pm uninstall --user 0 com.android.hotwordenrollment.okgoogle
pm uninstall --user 0 com.android.hotwordenrollment.xgoogle
pm uninstall --user 0 com.android.providers.calendar
pm uninstall --user 0 com.google.android.feedback
pm uninstall --user 0 com.google.android.katniss
pm uninstall --user 0 com.google.android.marvin.talkback
pm uninstall --user 0 com.google.android.syncadapters.calendar
pm uninstall --user 0 com.google.android.tts
pm uninstall --user 0 com.google.android.tv
pm uninstall --user 0 com.google.android.tvlauncher
pm uninstall --user 0 com.google.android.tvrecommendations
pm uninstall --user 0 com.google.android.videos
pm uninstall --user 0 com.netflix.ninja
pm uninstall --user 0 com.nvidia.bbciplayer
pm uninstall --user 0 com.nvidia.bbciplayer.launch
pm uninstall --user 0 com.nvidia.bbciplayer.launchsport
pm uninstall --user 0 com.nvidia.benchmarkblocker
pm uninstall --user 0 com.nvidia.feedback
pm uninstall --user 0 com.nvidia.tegrazone3
pm uninstall --user 0 com.plexapp.android
pm uninstall --user 0 com.plexapp.mediaserver.smb
pm uninstall --user 0 com.zattoo.player
```

## More aggressive debloating

Source: [https://florisse.nl/shield-debloat/](https://florisse.nl/shield-debloat/)

### Nvidia shit

```
pm uninstall -k --user 0 com.nvidia.ocs
pm uninstall -k --user 0 com.nvidia.diagtools
pm uninstall -k --user 0 com.nvidia.shieldtech.hooks
pm uninstall -k --user 0 com.nvidia.feedback
pm uninstall -k --user 0 com.nvidia.shield.registration
pm uninstall -k --user 0 com.nvidia.SHIELD.Platform.Analyser
pm uninstall -k --user 0 com.nvidia.shield.remote.server
pm uninstall -k --user 0 com.nvidia.shieldtech.proxy
pm uninstall -k --user 0 com.nvidia.factorybundling
pm uninstall -k --user 0 com.nvidia.shieldbeta
pm uninstall -k --user 0 com.nvidia.shield.registration
pm uninstall -k --user 0 com.nvidia.shield.nvcustomize
pm uninstall -k --user 0 com.nvidia.NvCPLUpdater
pm uninstall -k --user 0 com.nvidia.benchmarkblocker
pm uninstall -k --user 0 android.autoinstalls.config.nvidia
pm uninstall -k --user 0 com.nvidia.hotwordsetup
pm uninstall -k --user 0 com.nvidia.enhancedlogging
pm uninstall -k --user 0 com.nvidia.shield.ask
pm uninstall -k --user 0 com.nvidia.stats
pm uninstall -k --user 0 com.nvidia.shield.appselector
pm uninstall -k --user 0 com.nvidia.beyonder.server
pm uninstall -k --user 0 com.nvidia.developerwidget
pm uninstall -k --user 0 com.nvidia.NvAccSt
pm uninstall -k --user 0 com.nvidia.shield.remotediagnostic
```

### Android shit

```
pm uninstall -k --user 0 com.android.gallery3d
pm uninstall -k --user 0 com.android.dreams.basic
pm uninstall -k --user 0 com.android.printspooler
pm uninstall -k --user 0 com.android.feedback
pm uninstall -k --user 0 com.android.keychain
pm uninstall -k --user 0 com.android.cts.priv.ctsshim
pm uninstall -k --user 0 com.android.cts.ctsshim
pm uninstall -k --user 0 com.android.providers.calendar
pm uninstall -k --user 0 com.android.providers.contacts
pm uninstall -k --user 0 com.android.se
```

### Google shit

```
pm uninstall -k --user 0 com.google.android.speech.pumpkin
pm uninstall -k --user 0 com.google.android.tts
pm uninstall -k --user 0 com.google.android.videos
pm uninstall -k --user 0 com.google.android.tvrecommendations
pm uninstall -k --user 0 com.google.android.syncadapters.calendar
pm uninstall -k --user 0 com.google.android.backuptransport
pm uninstall -k --user 0 com.google.android.partnersetup
pm uninstall -k --user 0 com.google.android.inputmethod.korean
pm uninstall -k --user 0 com.google.android.inputmethod.pinyin
pm uninstall -k --user 0 com.google.android.apps.inputmethod.zhuyin
pm uninstall -k --user 0 com.google.android.tv
pm uninstall -k --user 0 com.google.android.tv.frameworkpackagestubs
pm uninstall -k --user 0 com.google.android.tv.bugreportsender
pm uninstall -k --user 0 com.google.android.backdrop
pm uninstall -k --user 0 com.google.android.leanbacklauncher.recommendations
pm uninstall -k --user 0 com.google.android.feedback
```

### Even more

```
pm uninstall -k --user 0 com.plexapp.mediaserver.smb
pm uninstall -k --user 0 com.netflix.ninja
pm uninstall -k --user 0 com.amazon.amazonvideo.livingroom
pm uninstall -k --user 0 com.amazon.amazonvideo.livingroom.nvidia
pm uninstall -k --user 0 com.google.android.youtube.tvmusic
pm uninstall -k --user 0 com.google.android.play.games
```
