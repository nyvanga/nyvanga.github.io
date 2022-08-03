# ZFS

[GPL Violations Related to Combining ZFS and Linux](https://sfconservancy.org/blog/2016/feb/25/zfs-and-linux/)

[Debian Bookworm Root on ZFS](https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/Debian%20Bookworm%20Root%20on%20ZFS.html)

## Rescue Debian install

Boot Debian live

Add `contrib` to `/etc/apt/sources.list`

```
sudo -i

apt update

apt install openssh-server debootstrap gdisk zfsutils-linux
```
UEFI boot stuff:
```
apt install grub-efi-amd64 shim-signed
```

Mount everything under `mnt`:
```
zpool export -a
zpool import -f -N -R /mnt rpool
zpool import -f -N -R /mnt bpool
zfs mount rpool/ROOT/debian
zfs mount -a
```

Chroot into the mounted environment:
```
mount --make-private --rbind /dev  /mnt/dev
mount --make-private --rbind /proc /mnt/proc
mount --make-private --rbind /sys  /mnt/sys
mount -t tmpfs tmpfs /mnt/run
mkdir /mnt/run/lock
chroot /mnt /bin/bash --login
mount /boot/efi
```

When done, exit chroot:

```
exit
```

Then umount:

```
mount | grep -v zfs | tac | awk '/\mnt/ {print $3}' | xargs -i{} umount -lf {}

zpool export -a
```

Probably going to return: `cannot export 'rpool': pool is busy`

If this fails for rpool and in initramfs, mounting it on boot may fail and you may need to `zpool import -f rpool`, then exit in the initamfs prompt.

```
reboot
```

## Add new disks to a pool

As always super simple with ZFS!

To new disks, added as a mirror vdev to existing pool:
```
$ zpool add <pool name> mirror <partition 1> <partition 2>
```
Done!

## Replace device in mirror or raidz

[zpool replace man page](https://openzfs.github.io/openzfs-docs/man/8/zpool-replace.8.html)

Once a disk has failed, remove it and replace it with a new one.

**TAKE CARE** to remove the right disk.

Then partition the disk so it fits with the setup of the existing mirror or raidz.

Once that is done it is time to replace the old device (typically one or more partions).

Run `zpool list -v` to get a view of your running setup:
```
$ zpool list -v
NAME                                                 SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
bpool                                                960M  48.7M   911M        -         -     0%     5%  1.00x  DEGRADED  -
  mirror                                             960M  48.7M   911M        -         -     0%  5.07%      -  DEGRADED
    some-device-name-part3                              -      -      -        -         -      -      -      -  ONLINE  
    id-number-of-old-part3                              -      -      -        -         -      -      -      -  UNAVAIL 
rpool                                               5.89T  4.32T  1.57T        -         -     0%    73%  1.00x  DEGRADED  -
  mirror                                            1.36T  1.13T   233G        -         -     0%  83.3%      -  DEGRADED
    some-device-name-part4                              -      -      -        -         -      -      -      -  ONLINE  
    id-number-of-old-part4                              -      -      -        -         -      -      -      -  UNAVAIL 
```

The two `UNAVAIL` devices are the ones to be replaced.

So run:
```
zpool replace bpool id-number-of-old-part3 new-device-part3
```
And
```
zpool replace rpool id-number-of-old-part4 new-device-part4
```
No output means success (I think).

Then check with `zpool list -v`
```
$ zpool list -v
NAME                                                   SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
bpool                                                  960M  49.6M   910M        -         -     0%     5%  1.00x    ONLINE  -
  mirror                                               960M  49.6M   910M        -         -     0%  5.16%      -  ONLINE  
    some-device-name-part3                                -      -      -        -         -      -      -      -  ONLINE  
    new-device-part3                                      -      -      -        -         -      -      -      -  UNAVAIL 
rpool                                                 5.89T  4.32T  1.57T        -         -     0%    73%  1.00x  DEGRADED  -
  mirror                                              1.36T  1.13T   233G        -         -     0%  83.3%      -  DEGRADED
    some-device-name-part4                                -      -      -        -         -      -      -      -  ONLINE  
    replacing                                             -      -      -        -         -      -      -      -  DEGRADED
      id-number-of-old-part4                              -      -      -        -         -      -      -      -  UNAVAIL 
      new-device-part4                                    -      -      -        -         -      -      -      -  ONLINE  
```
In this example the smaller partition was replaced emmidiatly, while the larger one took some time to synchronize.

Now if there are also multiple UEFI partitions, these needs manual work to syncrhonize.

Something along the lines of:
```
dd if=some-device-name-part2 of=new-device-part2
efibootmgr -c -g -d new-device-part2 -p 2 -L "bootdisk-2" -l '\EFI\debian\grubx64.efi'
```

## Replacing with larger disks

Example: replacing 2 disks in mirror with two egually sized but larger disks.

Partition disks like the old, only the main "data partiton" should use maximum size available.

Do a replace like above. ZFS is awesome and alows you to replace a device in a raid vdev with a larger one. It will just only use the part of it, equal to the old disk sizes.

Once all disks are replaced with larger ones (and completely resilvered) run a `zpool list -v`. It will probably report that your `mirror` is the old (small) size.

You can also check the "available" expandsize:
```
$ zpool get expandsize
NAME   PROPERTY    VALUE     SOURCE
bpool  expandsize  -         -
rpool  expandsize  1.8T      -
```

Check that `autoexpand` is enabled:
```
$ zpool get autoexpand
NAME   PROPERTY    VALUE   SOURCE
bpool  autoexpand  off     default
rpool  autoexpand  off     local
```

If not enabled, change with:
```
$ zpool set autoexpand=on rpool
```

Now ZFS will do autoexpanding when the devices are imported. So do the import:
```
$ zpool online -e rpool new-larger-device1-part4 new-larger-device2-part4
```

Check with a `zpool list -v` and the size should be included in the pool.

Or check that the expansize has disapeared:
```
$ zpool get expandsize
NAME   PROPERTY    VALUE     SOURCE
bpool  expandsize  -         -
rpool  expandsize  -         -
```
