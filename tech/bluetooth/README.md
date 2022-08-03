# Bluetooth

## Bluetooth pairing from command line

1. Get bluetoothctl:
```
apt install bluez
```
2. Start the bluetoothctl console:
```
bluetoothctl
```
3. Turn the power to the controller:
```
power on
```
4. Get the MAC Address of the device with which to pair:
```
devices
```
5. Enter device discovery mode with:
```
scan on
```
6. To do the pairing (tab completion works).
```
pair <MAC Address>
```
7. To register the device as a trusted device, so it will be re-connected after boot/reboot:
```
trust <MAC Address>
```
8. Finally, to establish a connection:
```
connect <MAC address>
```