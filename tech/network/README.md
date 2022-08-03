# Network

## Acronyms

Acronym                 | Speed      | Description
------------------------|------------|----------
FE                      | 100 Mbit/s | [Fast Ethernet](https://en.wikipedia.org/wiki/Fast_Ethernet)
GbE or GigE             | 1 Gbit/s   | [Gigabit Ethernet](https://en.wikipedia.org/wiki/Gigabit_Ethernet)
10GE, 10GbE, or 10 GigE | 10 Gbit/s  | [10 Gigabit Ethernet](https://en.wikipedia.org/wiki/10_Gigabit_Ethernet)

## Check Listening Ports with `netstat`

([source](https://linuxize.com/post/check-listening-ports-linux/#check-listening-ports-with-netstat))

`netstat` is a command-line tool that can provide information about network connections.

To list all TCP or UDP ports that are being listened on, including the services using the ports and the socket status use the following command:

```
sudo netstat -tunlp
```

The options used in this command have the following meaning:

- `-t` - Show TCP ports.
- `-u` - Show UDP ports.
- `-n` - Show numerical addresses instead of resolving hosts.
- `-l` - Show only listening ports.
- `-p` - Show the PID and name of the listener’s process. This information is shown only if you run the command as root or sudo user.

The output will look something like this:

```
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      445/sshd            
tcp        0      0 0.0.0.0:25              0.0.0.0:*               LISTEN      929/master          
tcp6       0      0 :::3306                 :::*                    LISTEN      534/mysqld          
tcp6       0      0 :::80                   :::*                    LISTEN      515/apache2         
tcp6       0      0 :::22                   :::*                    LISTEN      445/sshd            
tcp6       0      0 :::25                   :::*                    LISTEN      929/master          
tcp6       0      0 :::33060                :::*                    LISTEN      534/mysqld          
udp        0      0 0.0.0.0:68              0.0.0.0:*                           966/dhclient  
```

The important columns in our case are:

- `Proto` - The protocol used by the socket.
- `Local Address` - The IP Address and port number on which the process listen to.
- `PID/Program name` - The PID and the name of the process.

If you want to filter the results, use the grep command . For example, to find what process listens on TCP port 22 you would type:

```
sudo netstat -tnlp | grep :22
```

The output shows that on this machine port 22 is used by the SSH server:

```
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      445/sshd
tcp6       0      0 :::22                   :::*                    LISTEN      445/sshd
```

If the output is empty it means that nothing is listening on the port.

You can also filter the list based on criteria, for example, PID, protocol, state, and so on.

`netstat` is obsolete and replaced with ss and ip , but still it is of the most used commands to check network connections.

## Add extra IP to a NIC

To add extra IP's on a NIC just add lines in `/etc/network/interfaces` with:

```
<nic-name>:<virtual-nic-number>
```
Example (Virtual NIC number `1` on nic `eth0`):
```
auto eth0:1
allow-hotplug eth0:1
iface eth0:1 inet static
... rest of config
```
The number of the virtual NIC can be chosen freely, but needs to be unique.

To add extra VLAN's on a NIC just add lines in `/etc/network/interfaces` with:
```
<nic-name>.<vlan-id>
```
Example (VLAN id `3` on nic `eth0`):
```
auto eth0.3
allow-hotplug eth0.3
iface eth0.3 inet static
... rest of config
```

Because there can be only one "real" default gateway, to configure special gateways on extra IP's or VLAN's do:
```
auto eth0:3
allow-hotplug eth0:3
iface eth0:3 inet static
	address 192.168.14.4
	netmask 255.255.255.0
	post-up ip route add 192.168.14.0/24 dev eth0:3 src 192.168.14.4 table rt_tab
	post-up ip route add default via 192.168.14.1 dev eth0:3 table rt_tab
	post-up ip rule add from 192.168.14.4/32 table rt_tab
	post-up ip rule add to 192.168.14.4/32 table rt_tab
```
(Nic address `192.168.14.4`, Gateway `192.168.14.1`, Network `192.168.14.0/24`)

Now for this to work the route table named `rt_tab` needs to be declared in `/etc/iproute2/rt_tables`:
```
#
# reserved values
#
255     local
254     main
253     default
0       unspec
#
# local
#
#1      inr.ruhep
14 rt_tab
```
The number is abitrary, but needs to be unique. One approach is to use the same number as the VLAN id.
