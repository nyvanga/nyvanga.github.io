# Windows 11 on Linux KVM

[Source](https://getlabsdone.com/how-to-install-windows-11-on-kvm/)

Install some needed tools:
```
sudo apt install qemu-kvm bridge-utils virt-manager libosinfo-bin
```

Short list of steps:
1. `Create a new virtual machine`
1. `Local install media (ISO image or CDROM)`
1. `Choose ISO or CDROM install media:` -> `Browse...`
1. `Browse Local` -> Select the iso file -> `Forward`
1. `Memory:` -> 16384
1. `CPUs:` -> 4
1. `Forward`
1. `Customize configuration before install` -> `Finish`
   1. `Overview` -> `Firmware:` -> `UEFI x86_64: /usr/share/OVMF/OVMF_CODE_4M.secboot.fd`
   1. `CPUs` -> `Configuration` -> `Topology` -> `Manually set CPU topology`
      1. `Sockets:` -> 1
      1. `Cores:` -> 2
      1. `Threads:` -> 2
   1. `Boot Options` -> `Boot device order` -> `SATA CDROM 1` at the top
   1. `SATA Disk 1` -> `Disk bus:` -> `VirtIO`
   1. `Add Hardware` -> `Storage`
      1. `Select or create custom storage` -> virtIO.iso
      1. `Device type:` -> `CDROM device`
      1. `Advanced options` -> `Readonly:` check
   1. `NIC :mac...` -> `Device model:` -> `virtio`
   1. `Tablet` -> right click -> `Remove Hardware`
   1. `Add Hardware` -> `Graphics`
      1. `Type:` -> `VNC server`
   1. `TPM vNone` -> `Advanced options`
      1. `Model:` -> `TIS`
      1. `Version:` -> `2.0`

## TPM

[Source](https://getlabsdone.com/how-to-enable-tpm-and-secure-boot-on-kvm/)

Software TPM:
```
sudo apt install swtpm-tools
```

Check that it responds:
```
swtpm --version
```

Enable secure-boot/UEFI on KVM:
```
sudo apt install ovmf
```
