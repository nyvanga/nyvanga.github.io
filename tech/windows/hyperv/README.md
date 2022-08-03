# Hyper-V NAT network

[Set up a NAT network](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/setup-nat-network)

1. Create new internal switch
```
New-VMSwitch -SwitchName "NAT Switch" -SwitchType Internal
```

2. Get the `ifIndex` of the new switch
```
PS C:\> Get-NetAdapter

Name                  InterfaceDescription               ifIndex Status       MacAddress           LinkSpeed
----                  --------------------               ------- ------       ----------           ---------
vEthernet (intSwitch) Hyper-V Virtual Ethernet Adapter        24 Up           00-15-5D-00-6A-01      10 Gbps
Wi-Fi                 Marvell AVASTAR Wireless-AC Net...      18 Up           98-5F-D3-34-0C-D3     300 Mbps
Bluetooth Network ... Bluetooth Device (Personal Area...      21 Disconnected 98-5F-D3-34-0C-D4       3 Mbps
```

3. Set the IP range of the switch
```
New-NetIPAddress -IPAddress 192.168.42.1 -PrefixLength 24 -InterfaceIndex <ifIndex>
```

4. Configure NAT networking
```
New-NetNat -Name NATNet -InternalIPInterfaceAddressPrefix 192.168.42.0/24
```