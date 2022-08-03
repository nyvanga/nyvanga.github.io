# Windows update

## Trouble

- [Windows Updates: Package failed to be changed to the Installed state. Status: 0x800f0922](https://answers.microsoft.com/en-us/windows/forum/all/windows-updates-package-failed-to-be-changed-to/4154ea0a-8426-421c-85cc-e30b54230fec)

## Update broken

Some times (especially when a rollback to a restore point) Windows Update gets into a funky state.
To reset Windows update, and force it to rediscover which updates are actually needed, run:

```
net stop bits

net stop wuauserv

ren %systemroot%\softwaredistribution softwaredistribution.bak

ren %systemroot%\system32\catroot2 catroot2.bak

net start bits

net start wuauserv

```
[Fejlfinding af problemer under opdatering af Windows](https://support.microsoft.com/da-dk/windows/fejlfinding-af-problemer-under-opdatering-af-windows-188c2b0f-10a7-d72f-65b8-32d177eb136c)
