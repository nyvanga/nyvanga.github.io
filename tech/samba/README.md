# Samba

Mount:
```
mount -t cifs //<ip or host>/<share> <mount point>/ -o username=<username>,password=<password>
```
Or:
```
mount.cifs //<ip or host>/<share> <mount point>/ -o username=<username>,password=<password>
```

## Trouble with old versions

Setting the version might help:
```
mount -t cifs //<ip or host>/<share> <mount point>/ -o username=<username>,password=<password>,vers=<1.0, 2.0, 3.0>
```
Usually version 1.0 is not allowed, since it is super insecure!
([Source](https://stackoverflow.com/questions/51676885/mount-error95-cifs-utils-on-ubuntu-12-04))