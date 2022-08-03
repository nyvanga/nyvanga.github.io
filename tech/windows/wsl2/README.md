# WSL info

- [Comparing WSL 1 and WSL 2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions)
- [WSL 2 Settings](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#wsl-2-settings)
- [Configure per distro launch settings with wslconf](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#configure-per-distro-launch-settings-with-wslconf)
- [wslu - A collection of utilities for WSL](https://github.com/wslutilities/wslu)
- [Pengwin - NONFREE!!! Linux distribution optimized for WSL](https://github.com/WhitewaterFoundry/Pengwin)
   - [IntelliJ 2021 Java](https://github.com/WhitewaterFoundry/Pengwin/wiki/IntelliJ-2021-Java)
- [WSL-Programs - A community powered list of programs that work](https://github.com/ethanhs/WSL-Programs)
- [Bash On Ubuntu On Windows](https://github.com/abergs/ubuntuonwindows)

## Trouble

- [WSL2 corrupts ext4 filesystem](https://github.com/microsoft/WSL/issues/5895)
- [WSL 2 DNS not working](https://github.com/microsoft/WSL/issues/4855)

# Install

1. Enable the Windows Subsystem for Linux (Powershell as admin):
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

2. Enable Virtual Machine feature (Powershell as admin):
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

3. Restart your machine to complete the WSL install.

4. Download the Linux kernel update package: https://aka.ms/wsl2kernel ([direct download](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)).

5. Set WSL 2 as your default version (as admin): 
```
wsl --set-default-version 2
```

6. Goto Windows store and install your favorite (Debian) distro. (Direct download [here](https://docs.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions).)

## More info:
- https://aka.ms/wsl2

# Upgrade Linux kernel

Suprise, surprise. This doesn't happen automatically.

1. Download the Linux kernel update package: https://aka.ms/wsl2kernel ([direct download](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)).

2. Restart your machine, and your WSL should be running the new kernel version.

# X Server and UI apps from WSL2

This is just a shit show! But here goes... ([Info on VSOCK and WSL2](https://x410.dev/cookbook/wsl/using-x410-with-wsl2/))

Main guide: [Using IntelliJ with Windows Subsystem for Linux 2](https://www.tomaszmik.us/2020/01/26/intellij-on-wsl/).


## Run the X Server with Cygwin/X

1. Install XWin stuff with Cygwin/X as described [here](https://x.cygwin.com/docs/ug/setup.html#setup-cygwin-x-installing)!

2. Run this command:
```
C:\cygwin64\bin\run.exe --quote /usr/bin/bash.exe -l -c "cd; exec /usr/bin/startxwin -- -ac -multiwindow -wgl -clipboard -noprimary -listen tcp"
```

## Allow Inbound connections to `C:\cygwin64\bin\xwin.exe` in the Windows Firewall

1. Launch the above command, and the first time it will ask for Firewall permissions. Only allow `Private networks`.
1. This will create block rules on `Public networks` which needs to be removed or disabled.
1. Create a new firewall rule:
   1. Right click on `Inbound Rules`
   1. Choose `New Rule...`
   1. Leave it at `Program` and choose `Next >`
   1. Set `This program path` to `C:\cygwin64\bin\xwin.exe`
   1. Leave it at `Allow the connection` and click `Next >`
   1. Only `Public` should be chosen and click `Next >`. 
   1. Add a `Name` fx. `xwin (WSL2)` and click `Finish`.
   1. Open the newly created rule and go to `Scope`.
   1. Change `Remote IP address` to `These IP addresses:`.
   1. Click `Add...`.
   1. Input all reserved private IPv4 network ranges. Yes it sucks! But WSL moves around from reboot to reboot!

Local IP ranges:
```
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

WSL Windows Toolbar Launcher has a good guide on the [Firewall rules](https://github.com/cascadium/wsl-windows-toolbar-launcher#firewall-rules).

## Set the `DISPLAY` ENV variable in Wsl2, preferably in the `~/.bashrc` file

```
export WSL_HOST=$(grep ^nameserver /etc/resolv.conf | cut -d' ' -f2)
export DISPLAY=${WSL_HOST}:0.0
```

# Vmmem using all memory and CPU?

We need to set some reasonable resource constraints on what WSL2 can actually use. Fortunately, that’s as simple as going to `c:\users\*your your profile name*` and creating a `.wslconfig` file. On my setup, a MSI Prestige 15 with a 10710u 6-core processor and 16GB of RAM, mine looks like this:
```
[wsl2]
memory=4GB # Limits VM memory in WSL 2 to 4 GB
processors=5 # Makes the WSL 2 VM use two virtual processors

```
And then, from Powershell with admin rights, restart WSL2 by typing:
```
Restart-Service LxssManager

```
[Source](https://medium.com/@lewwybogus/how-to-stop-wsl2-from-hogging-all-your-ram-with-docker-d7846b9c5b37)

# Launching Docker Desktop WSL2
```
PS C:\WINDOWS\system32> wsl -l -v
  NAME                   STATE           VERSION
* Debian                 Stopped         2
  docker-desktop-data    Stopped         2
  docker-desktop         Stopped         2
PS C:\WINDOWS\system32> wsl -l -v
  NAME                   STATE           VERSION
* Debian                 Stopped         2
  docker-desktop-data    Stopped         2
  docker-desktop         Running         2
PS C:\WINDOWS\system32> wsl -l -v
  NAME                   STATE           VERSION
* Debian                 Stopped         2
  docker-desktop-data    Running         2
  docker-desktop         Running         2
PS C:\WINDOWS\system32> wsl -l -v
  NAME                   STATE           VERSION
* Debian                 Stopped         2
  docker-desktop-data    Running         2
  docker-desktop         Running         2
PS C:\WINDOWS\system32> wsl -l -v
  NAME                   STATE           VERSION
* Debian                 Running         2
  docker-desktop-data    Running         2
  docker-desktop         Running         2
```
