# Xbox One Wireless controller

The wireless dongle actually communicates through wifi, but on a special M$ protocol (obviously).

## User mode driver

Project kind of abandoned, in favor of a kernel driver.

[https://github.com/medusalix/xow](https://github.com/medusalix/xow)

xow works with the wireless dongle, but uses [100% CPU](https://github.com/medusalix/xow/issues/181) because of a bug in [libusb 1.0.24](https://github.com/medusalix/xow/issues/141).

## Kernel driver

Which does **NOT** support the wireless dongle!

[https://github.com/paroj/xpad](https://github.com/paroj/xpad)