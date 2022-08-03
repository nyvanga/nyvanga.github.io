# Hardware sensors

[lm-sensors](https://github.com/lm-sensors)

## ASUS ROG STRIX X570-I

Put `acpi_enforce_resources=lax` in `/etc/default/grub` on the `GRUB_CMDLINE_LINUX_DEFAULT="... other defaults ..."` (space separated).

Remember to run: `update-grub`

[No CPU FAN sensor monitoring on ASUS ROG STRIX X570-I](https://github.com/lm-sensors/lm-sensors/issues/220)

## Gigabyte Aorus B550M PRO-P

Put `acpi_enforce_resources=lax` in `/etc/default/grub` on the `GRUB_CMDLINE_LINUX_DEFAULT="... other defaults ..."` (space separated).

Remember to run: `update-grub`

Gigabyte Aorus B550M PRO-P: [ITE IT8688E not detected](https://github.com/lm-sensors/lm-sensors/issues/154)

Run: `sudo modprobe it87 force_id=0x8628`

Or: `echo 'options it87 force_id=0x8628 >/etc/modprobe.d/it87.conf`
