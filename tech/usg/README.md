# Help

[https://help.ubnt.com/hc/en-us/articles/360005460813-UniFi-USG-Advanced-Policy-Based-Routing-](https://help.ubnt.com/hc/en-us/articles/360005460813-UniFi-USG-Advanced-Policy-Based-Routing-)

[https://community.ubnt.com/t5/UniFi-Routing-Switching/USG-static-route-with-dual-WAN-load-balancing-bug-feature/td-p/2405120](https://community.ubnt.com/t5/UniFi-Routing-Switching/USG-static-route-with-dual-WAN-load-balancing-bug-feature/td-p/2405120)

[https://help.ubnt.com/hc/en-us/articles/360001004034-UniFi-Best-Practices-for-Managing-Chromecast-Google-Home-on-UniFi-Network](https://help.ubnt.com/hc/en-us/articles/360001004034-UniFi-Best-Practices-for-Managing-Chromecast-Google-Home-on-UniFi-Network)

# Scheduled upgrades

Settings -> Services -> Scheduled upgrades -> Create new scheduled upgrade


# Adopt a USG into an Existing Network

1. Connect a computer to the LAN NIC (LAN port) of the USG. It will obtain a 192.168.1.x IP from DHCP.

2. SSH into 192.168.1.1 using username and password combination of ubnt / ubnt.

3. For this example, the controller is on 192.168.10.3/24, so let's change the USG’s LAN IP to 192.168.10.1.

4. In the SSH session, run the following:
```
configure
set interfaces ethernet eth1 address 192.168.10.1/24
delete interfaces ethernet eth1 address 192.168.1.1/24
commit
```
   Now the USG’s LAN IP is 192.168.10.1/24. The SSH session will drop.  

5. Plug the USG’s LAN into the network with the controller at 192.168.10.3

6. Go to the UniFi Controller and adopt it.

7. Go into the network and setup DHCP, otherwise the USG will continue to serve ip's from the 192.168.1.0/24 range.


# Adopt a device into an Existing Network

1. Connect to device: `ssh ubnt@<ip address of dev>`

2. Password is default: `ubnt`

3. Run: `set-inform http://192.168.10.3:8080/inform`

Now the device shows up in the controller software, so click adopt.

4. While it is "adopting", rerun the command in step 3.

# DNS forwarding + static records

## Check
```
  show dns forwarding nameservers
  show dns forwarding statistics
  configure + show service dns
  configure + show system name-server
```

## Setup
```
configure
edit service dns
set forwarding name-server 1.1.1.1
set forwarding name-server 1.0.0.1
set forwarding name-server 8.8.8.8
set forwarding name-server 8.8.4.4
set forwarding options host-record=some-domain.com,192.168.10.7
set forwarding options host-record=other-domain.dk,192.168.10.9
set forwarding options host-record=www.homepage.net,192.168.10.11
commit
save
exit
```

# Check load balancing
```
show load-balance status
show load-balance watchdog
```

# VLAN's that do NOT use load balancing

[https://help.ubnt.com/hc/en-us/articles/360005460813-UniFi-USG-Advanced-Policy-Based-Routing-](https://help.ubnt.com/hc/en-us/articles/360005460813-UniFi-USG-Advanced-Policy-Based-Routing-)

## To find the gateways of the individual WAN interfaces:

```
show load-balance status
```

## Setup exception rules

```
configure

# Excluding inter LAN/VLAN traffic from load balancing
set firewall group address-group ALL_LANS address 192.168.10.0/24
set firewall group address-group ALL_LANS address 192.168.30.0/24
set firewall group address-group ALL_LANS address 192.168.40.0/24
set firewall modify LOAD_BALANCE rule 2500 action accept
set firewall modify LOAD_BALANCE rule 2500 source group address-group ALL_LANS
set firewall modify LOAD_BALANCE rule 2500 destination group address-group ALL_LANS

# Excluding guest inter LAN/VLAN traffic from load balancing
set firewall group address-group GUEST_LANS address 192.168.60.0/24
set firewall group address-group GUEST_LANS address 192.168.70.0/24
set firewall group address-group GUEST_LANS address 192.168.80.0/24
set firewall modify LOAD_BALANCE rule 2510 action accept
set firewall modify LOAD_BALANCE rule 2510 source group address-group GUEST_LANS
set firewall modify LOAD_BALANCE rule 2510 destination group address-group GUEST_LANS

# Streaming network WAN only (USG3 = eth0)
# Next-hop IP is the WAN router/modem IP
set protocols static table 30 route 0.0.0.0/0 next-hop 192.168.100.1
set firewall modify LOAD_BALANCE rule 2530 action modify
set firewall modify LOAD_BALANCE rule 2530 modify table 30
set firewall modify LOAD_BALANCE rule 2530 source address 192.168.30.0/24
set firewall modify LOAD_BALANCE rule 2530 protocol all

# Guest network WAN2 only (USG3 = eth2)
# Next-hop IP is the WAN2 router/modem IP
set protocols static table 40 route 0.0.0.0/0 next-hop 192.168.100.1
set firewall modify LOAD_BALANCE rule 2540 action modify
set firewall modify LOAD_BALANCE rule 2540 modify table 40
set firewall modify LOAD_BALANCE rule 2540 source address 192.168.40.0/24
set firewall modify LOAD_BALANCE rule 2540 protocol all

commit
save
exit
```

# Route tables
[https://help.ubnt.com/hc/en-us/articles/360005460813-UniFi-USG-Advanced-Policy-Based-Routing-](https://help.ubnt.com/hc/en-us/articles/360005460813-UniFi-USG-Advanced-Policy-Based-Routing-)

## Route where next hop is an interface:

Doesn't seem to work anymore. Use address instead.

```
set protocols static table 40 interface-route 0.0.0.0/0 next-hop-interface eth2
```

## Route where next hop is an address:
```
set protocols static table 45 route 0.0.0.0/0 next-hop 192.168.30.1
```


# Advanced config

[https://help.ubnt.com/hc/en-us/articles/215458888-UniFi-USG-Advanced-Configuration](https://help.ubnt.com/hc/en-us/articles/215458888-UniFi-USG-Advanced-Configuration)

1. Connect to the USG via SSH, and issue the needed commands.

```
configure
... config commands ...
commit
save
exit
```

2. Next is displaying the config. The following command displays the entire config in a JSON format:

```
mca-ctrl -t dump-cfg
```

Anlternativly the config can also be exported if preferred. The following example exports the output to the config.txt:

```
mca-ctrl -t dump-cfg > config.txt
```

3. Find the appropriate section with the custom changes in the config output.

4. It could be missing all the closing brackets (}) at the end to make it correct.
   If you look at the config output from the start, there is a certain format that is required for the file to be read correctly.
   Each node in a section must be separated by a comma (,), and it section must begin with an opening bracket ({) and finish with a closing one (}).
   Follow the existing format carefully.

5. It's recommended to validate the code once finished creating the config.gateway.json.
   There are a number of free options out there, jsonlint.com is used by the UBNT support team quite often.

6. After adding the config.gateway.json to the Controller site of your choosing, you can test it by running a "force provision" to the USG:

Unifi Controller: Devices > USG > Config > Manage Device > Force provision

# More

[https://community.ubnt.com/t5/UniFi-Routing-Switching/SSDP-Multicast-Across-VLANs/td-p/2295718](https://community.ubnt.com/t5/UniFi-Routing-Switching/SSDP-Multicast-Across-VLANs/td-p/2295718)

[https://community.ubnt.com/t5/UniFi-Routing-Switching/USG-Set-hostname-by-mac-address/m-p/2537378/thread-id/113776](https://community.ubnt.com/t5/UniFi-Routing-Switching/USG-Set-hostname-by-mac-address/m-p/2537378/thread-id/113776)