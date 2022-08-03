# Installing Debian

Get a DVD image and write to an USB stick.

Use [Rufus](https://rufus.ie/) to write the USB stick.

If you want a UEFI bootable Debian Linux, make sure:
1. Write the USB stick as UEFI bootable
1. Boot it with UEFI.

There is NO WAY to change this once installed.

__As of Debian 12 (Bookworm), firmware is included in the normal Debian installer images.__

So special images including firmware doesn't exist any more. Hurray!!!

See the Debian Wiki [non-free firmware](https://wiki.debian.org/Firmware) page for more information. 

## Current or stable DVD images (torrents)
- [Install DVD](https://cdimage.debian.org/cdimage/release/current/amd64/bt-dvd/)
- [Live DVD](https://cdimage.debian.org/cdimage/release/current-live/amd64/bt-hybrid/)

## Testing (weekly-builds) DVD images
- [Install DVD](https://cdimage.debian.org/cdimage/weekly-builds/amd64/iso-dvd/)
- [Live DVD](https://cdimage.debian.org/cdimage/weekly-live-builds/amd64/iso-hybrid/)

## Debian 11 bullseye

Missing `libappindicator3-1` problems. Well it is deprecated, so it should be missing, but is installable from here:
- [libappindicator3-1](https://packages.debian.org/buster/libappindicator3-1)
   - [libindicator3-7](https://packages.debian.org/buster/libindicator3-7)

Missing `libappindicator1` same but different:
- [libappindicator1](https://packages.debian.org/buster/libappindicator1)
   - [libindicator7](https://packages.debian.org/buster/libindicator7)
   - [libdbusmenu-gtk4](https://packages.debian.org/buster/libdbusmenu-gtk4)
