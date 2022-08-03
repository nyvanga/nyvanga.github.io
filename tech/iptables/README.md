# IP tables

## Xtables-addons

[Xtables-addons](https://inai.de/projects/xtables-addons/)

On Debian 11 Bullseye, most modules are already included, so install with:
```
sudo apt-get install xtables-addons-common libtext-csv-xs-perl
```

### xt_geoip module

[xt_geoip module](https://inai.de/projects/xtables-addons/geoip.php)

The geoip module needs a geoip database located in `/usr/share/xt_geoip`.

Scripts to download geoip CSV files, and build them into this database are included in the install.

Download and update database:
```
mkdir /tmp/xt_geoip
cd /tmp/xt_geoip
/usr/libexec/xtables-addons/xt_geoip_dl
mkdir -p /usr/share/xt_geoip
/usr/libexec/xtables-addons/xt_geoip_build -D /usr/share/xt_geoip/
rm -rf /tmp/xt_geoip
```

### Examples:

block incoming traffic from China
```
iptables -I INPUT -m geoip --src-cc CH -j DROP
```

block outgoing traffic to China
```
iptables -I OUTPUT -m geoip --dst-cc CH -j DROP
```

Basically the module can be added to any `iptables` with:
```
-m geoip --src-cc country[,country...] --dst-cc country[,country...]
```