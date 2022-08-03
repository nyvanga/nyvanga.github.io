# Install

## Install without a Micro$oft account

The trick is as always to install without network connectivity.

But with Windows 11 this has become harder... but not impossible.

So first of all when installing without network, Windows cannot verify the digital license.

Just ignore this by choosing `I don't have a product key`. This is not a problem, and will be resolved once the network is enabled, after install.

At the screen in the install, where Windows `demands` network, press `shift + F10`.

This will bring up a terminal window. Type the following and then enter:
```
OOBE\BYPASSNRO
```
This will cause the PC the reboot, and the install seems to start over.

Only difference will be that the network screen will now have an option `I don't have internet`.

Choose that option, and then `Continue with limited setup`.

Now you will be able to choose a local account name and password.

Hurray!
