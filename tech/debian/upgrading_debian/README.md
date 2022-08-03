# Upgrade Debian

Currently most relevant upgrade is [Bullseye to Bookworm](https://www.debian.org/releases/bookworm/amd64/release-notes/ch-upgrading.en.html)

The main aproach is being cautios, and considering if the install is pure Debian, or there are special things added.

Otherwise the quick and dirty route is:

## Check for obsolete packages

```
apt list '~o'
```

And remove if they are not essential:
```
apt purge '~o'
```

## Find and cleanup leftover configurations

```
find /etc -name '*.dpkg-*' -o -name '*.ucf-*' -o -name '*.merge-error'
```

## Final checks

Check that GPG is installed:
```
apt install gpgv
```

Check packages:
```
dpkg --audit
```

## Update package lists

Change `bullseye` to `bookworm` in all of:
```
/etc/apt/sources.list
/etc/apt/sources.list.d/*
```
Unless there is a good reason not to.

Special for `bookworm` is that [firmware has move to it's own component](https://www.debian.org/releases/bookworm/amd64/release-notes/ch-information.en.html#non-free-split), so to be safe add it to the "main" debian deb:
```
deb https://deb.debian.org/debian bullseye main contrib non-free
```
Like this
```
deb https://deb.debian.org/debian bookworm main contrib non-free non-free-firmware
```

Update, and watch out for failures:
```
apt update
```

## Prepare upgrade

It is recommended to record a script:
```
script -t 2>~/upgrade-bookworm1.time -a ~/upgrade-bookworm1.script
```
Increment the number every time, so they do not get overridden.

Check that there is enough free space:
```
apt -o APT::Get::Trivial-Only=true full-upgrade
```
It will fail with:
```
E: You don't have enough free space in /var/cache/apt/archives/.
```

## The upgrade

First one without installing new packages:
```
apt upgrade --without-new-pkgs
```

If that all looks good, go for full upgrade:
```
apt full-upgrade
```

When confident and ready, do a reboot!
