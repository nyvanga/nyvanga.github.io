# Wifi:

Wifi needs a special proparitary firmware that is not open-source and therefor not distributed by Debian.

## Install driver (Debian 10):

**NOTE!** To install backports versions of kernel and firmware do:

1. Add to /etc/apt/sources.list:
```
deb http://deb.debian.org/debian buster-backports main contrib non-free
```
2. Install latest kernel:
```
apt -t buster-backports install linux-image-amd64
```
3. Install firmware package:
```
apt -t buster-backports install firmware-iwlwifi
```
## Install driver (Debian 11):

On Debian 11 the kernel is already at 5.9 (at least), so all that is missing is the firmware package:
```
apt install firmware-iwlwifi
```

# Nvidia driver:

[https://wiki.debian.org/NvidiaGraphicsDrivers#Debian_10](https://wiki.debian.org/NvidiaGraphicsDrivers#Debian_10_.22Buster.22)

* Install driver (Debian 10):

```
apt -t buster-backports install nvidia-driver firmware-misc-nonfree
```

[https://wiki.debian.org/NvidiaGraphicsDrivers#Debian_11](https://wiki.debian.org/NvidiaGraphicsDrivers#Debian_11_.22Bullseye.22)

* Install driver (Debian 11):

```
apt install nvidia-driver firmware-misc-nonfree
```

## Secure boot DKMS trouble

The DKMS compile might end with errors like:
```
Job for systemd-modules-load.service failed because the control process exited with error code.
See "systemctl status systemd-modules-load.service" and "journalctl -xe" for details.
```

Checking those to the root cause seemed to be:
```
Lockdown: modprobe: unsigned module loading is restricted; see https://wiki.debian.org/SecureBoot
```
Long story short, you need to sign you driver files with a custom key, that is then added to the UEFI bios secure boot. (See below).

## PRIME Render Offload

When running on Debian 11 Bullseye, Optimus (Nvidia PRIME Render Offload) should be working out of the box. This is the feature where the desktop runs on the Intel GPU, and you can start individual programs running on the NVIDIA GPU.

* https://wiki.debian.org/NVIDIA%20Optimus#Using_NVIDIA_PRIME_Render_Offload 

## Steam 32-bit libs

If you are going to run Steam, make sure to install the 32-bit libraries
* https://wiki.debian.org/NvidiaGraphicsDrivers#multiarch-install

1. After installing the drivers, enable 32-bit multiarch and update your repository listing by running:
```
sudo dpkg --add-architecture i386 && sudo apt update
```
2. Afterwards, to install the 32-bit version of the NVIDIA libraries package, run:
```
sudo apt install nvidia-driver-libs:i386
```

#  Installing Steam

It is part of the `non-free` packages, so just make sure they are all enabled.

Then just install Steam:
```
apt install steam
```

https://wiki.debian.org/Steam#A64-bit_systems_.28amd64.29

# MOK (Machine Owner Key) and adding to UEFI secure boot

https://wiki.debian.org/SecureBoot#MOK_-_Machine_Owner_Key

1. Follow the procedure to add a MOK key.

2. Go to:
```
/lib/modules/<kernel-version>-amd64/updates/dkms
```

3. Sign all nvida .ko files. Or whatever you need.
```
/usr/lib/linux-kbuild-<kernel-version>/scripts/sign-file sha256 /root/MOK.priv /root/MOK.der <some-driver>.ko
```

Signing the nvidia-drivers made it load, and that made the xrandr cmd work, YAY!
```
xrandr --output <display> --brightness 0.5
```

All this capture in a script: [mok.sh](../../bin/mok.sh)

# System tray icons

If you have Debian 10 installed with Gnome you probably do not have system tray icons at the top right corner.
You need to install and enable ‘gnome-shell-extension-appindicator’.

1. (you might need to log out and back in again for gnome to see the extension)
```
sudo apt install gnome-shell-extension-appindicator
```
2. search tweaks in your Activities screen

3. Switch to Extensions

4. Enable Kstatusnotifieritem/appindicator support


# Mount points that actually works

The Gnome fancy-pants connect to Samba shares, ends up recreating changed files!?
And that causes set bits to be reset to default. BAD!!!

So add this to /etc/fstab:
```
//<IP or hostname>/<path to some share> /home/<username>/<mount path> cifs user,uid=<username>,gid=<groupname>,rw,noauto 0 0
```

This allows the user to mount the `<mount path>` within their home folder, whithout using sudo.
It will ask for password, but could also use a file with the password in plain text :-(

If there is an error like:
```
mount: /home/<username>/<mount path>: bad option; for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program.
```
It is because `cifs` is not installed:
```
sudo apt-get install cifs-utils
```

# Grub with custom splash screen

Open /etc/default/grub and add a new GRUB_BACKGROUND line:
```
GRUB_BACKGROUND=/path/to/your/bg.jpg
```

Update the grub configuration file:
```
sudo update-grub
```

# Login screen themes
Have seen some mentions of themes for GDM but also mentions that GDM can't be changed without recompile!?
So solution is to go `lightdm` which has tons of configuration posibilities:
```
sudo apt install lightdm
```
This will ask you to choose default display manager, so go ahead and choose lightdm. To choose again:
```
sudo dpkg-reconfigure lightdm
```
Some options needs to be changed immediatly.
1. Default session. Which is not GNOME, so if you use GNOME, and the theme doesn't allow changing session, you are screwed. Open `/etc/lightdm/lightdm.conf`:
```
[Seat:*]
user-session=gnome
```
2. By default you have to type your username. To get a dropdown menu with users open `/etc/lightdm/lightdm.conf`:
```
[Seat:*]
greeter-hide-users=false
```
3. To change the background edit `/etc/lightdm/lightdm-gtk-greeter.conf` (provided the greeter hasn't been changed):
```
[greeter]
background=<full path to png or svg file>
```
4. Using a high DPI display edit `/etc/lightdm/lightdm-gtk-greeter.conf` (provided the greeter hasn't been changed):
```
[greeter]
xft-dpi=261
```