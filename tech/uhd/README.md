# UHD display

## Unreadable Grub menu?

### The goto solution is to set a resolution, preferably 1024x768

1. Edit the /etc/default/grub file adding a line:
```
GRUB_GFXMODE=1024x768
```

This also openes the door for setting a custom background! Win win!? ;-)

### Or choose larger font.

So take your screen width (e.g. 3840 ~ 4K) divided by 80 characters = 3840 / 80 = 48

1. Choose a font, in this example I chose DejaVuSansMono.ttf

2. Convert the font in a format GRUB understands:
```
sudo grub-mkfont -s 48 -o /boot/grub/DejaVuSansMono.pf2 /usr/share/fonts/truetype/dejavu/DejaVuSansMono.ttf
```

3. Edit the /etc/default/grub file adding a line:
```
GRUB_FONT=/boot/grub/DejaVuSansMono.pf2
```

4. Update GRUB configuration with:
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

5. reboot.

## Unreadable Linux console?

Luckily, there’s a remedy for this. First, squint your eyes and login, and set the font directly with:
```
setfont Lat15-Terminus32x16
```

If you want to try other fonts, check under `/usr/share/consolefonts`.

To make that change permanent, edit `/etc/default/console-setup` and make the following three variables look like this:
```
CODESET="Uni3"
FONTFACE="Terminus"
FONTSIZE="32x16"
```
For some resaon it wouldn't work on Debian server unless `FONTSIZE` was:
```
FONTSIZE="16x32"

¯\_(ツ)_/¯
```

Now you can enjoy a readable font size and that will get applied on every boot.