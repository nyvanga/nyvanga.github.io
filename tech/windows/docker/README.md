# Docker for Windows:

- [Release notes](https://docs.docker.com/docker-for-windows/release-notes/)
- [Direct download link](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe)

# WSL + Docker

- [Setting Up Docker for Windows and WSL to Work Flawlessly](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly)

# Docker in a VirtualBox:

Download and install Docker ToolBox: https://docs.docker.com/toolbox/toolbox_install_windows/
(Ignore all the VM .iso file stuff)

Create a VirtualBox Linux. Your preferable flavor.

Create and extra NIC with a "Host-only Adapter"

Once your Linux is running change that NIC to have static IP. Typically in the 192.168.56.0/24 range.

Add that IP to: `C:\Windows\System32\drivers\etc\hosts`

And maybe your `~/.ssh/config`

Once Docker is installed on the Linux, create /etc/docker/daemon.json:
```
{
  "hosts": ["unix:///var/run/docker.sock", "tcp://0.0.0.0:2376"]
}
```
Restart docker daemon.

Setup and ENV variable: `DOCKER_HOST=tcp://<STATIC_IP_OF_LINUX>:2376`

Now both the docker and docker-compose commands runs against that VM.

Pretty much the same Docker for Windows does (when not using Windows docker images), only difference is that ports are not exposed on 127.0.0.1 but rather the static IP you chose.