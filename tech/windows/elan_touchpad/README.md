# ELAN Touchpad (Crap!)

The Lenovo Thinkbook 14IIL has a touchpad, that doesn't work with Windows 11.

It works, but the configuration page in the driver is inaccessible.

## Two-finger-tap right-click

The worst part of that is that the two-finger-tap is missing.

Enable that with:
```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Elantech\SmartPad]
"Tap_Two_Finger"=dword:00000001
"Tap_Two_Finger_Enable"=dword:00000001
```
