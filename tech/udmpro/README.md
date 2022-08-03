# UDM Pro remote access

## Disabling

Login on directly on the box, since you cannot disable remote access while using it!

REMEMBER: You need to use the "Ubiquiti Account" user, since the option is NOT available for local users.

1. Goto `Settings -> System`
   - [https://box-ip-address/settings/system](https://box-ip-address/settings/system)
1. Flip the `Remote Access` toggle

Done!

## Enabling

Login directly on the box, since remote access is probably disabled!

REMEMBER: You need to use the "Ubiquiti Account" user, since the option is NOT available for local users.

1. Goto `Settings -> System`
   - [https://box-ip-address/settings/system](https://box-ip-address/settings/system)
1. Flip the `Remote Access` toggle

It will ask for `username/email`, `password` and `2FA`, so have those ready.

Done!

# Create local user

1. Goto `Users`
   - [https://box-ip-address/users](https://box-ip-address/users)
1. Click `Add User`
1. Choose `Role` to be `Administrator`
1. And `Account Type` to be `Local Access Only`
1. Fill in `First Name`, `Last Name`, `Local Username` and `Local Password`
1. Click `Add`

This is now a user that can login locally, without having to authenticate against [unifi.ui.com](https://unifi.ui.com).

# Port-forwarding crap

- [UniFi - USG/UDM: Port Forwarding Configuration and Troubleshooting](https://help.ui.com/hc/en-us/articles/235723207-UniFi-USG-UDM-Port-Forwarding-Configuration-and-Troubleshooting)
- [UDM Pro - port forwards not working on multiple WAN interfaces?](https://community.ui.com/questions/UDM-Pro-port-forwards-not-working-on-multiple-WAN-interfaces/904a40f6-7427-4066-8f97-4aff595f28cf)
- [UDM-Pro and dual WAN (per-VLAN or even WiFi Network)](https://community.ui.com/questions/UDM-Pro-and-dual-WAN-per-VLAN-or-even-WiFi-Network/39015cbb-77b3-4eac-af5b-788b20358c2f)

# Install on-boot

[on-boot-script-2.x](https://github.com/unifi-utilities/unifios-utilities/tree/main/on-boot-script-2.x)

The description suggests curl'ing a file directly to `sh`, but I would recommend this:

```
wget https://raw.githubusercontent.com/unifi-utilities/unifios-utilities/HEAD/on-boot-script-2.x/remote_install.sh
```

Look thoroughly through the `remote_install.sh` script. Only then:
```
chmod +x remote_install.sh
./remote_install.sh
```

This adds some scripts to install SNI plug-ins. Don't know what that is for, so remove them:

```
rm /data/on_boot.d/*
```

# Install on-boot (legacy)

[on-boot-script](https://github.com/boostchicken/udm-utilities/tree/master/on-boot-script)

Every time the UDM has been upgraded:

1. Get into the unifios shell on your udm

```bash
unifi-os shell
```

2. Download [udm-boot_1.0.5_all.deb](https://github.com/unifi-utilities/unifios-utilities/tree/main/on-boot-script/packages/) and install it and go back to the UDM

```bash
curl -L https://github.com/unifi-utilities/unifios-utilities/raw/main/on-boot-script/packages/udm-boot_1.0.5_all.deb -o udm-boot_1.0.5_all.deb
dpkg -i udm-boot_1.0.2_all.deb
exit
```

# Take control of those noisy fans...

[ubnt-fan-speed](https://github.com/heXeo/ubnt-fan-speed)

Old, but still works on UnifiOS 2.4.x
