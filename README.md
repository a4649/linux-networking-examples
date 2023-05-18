# linux-networking-examples
Linux networking commands and configuration examples cheat-sheet

Based on https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf

### 1. show / debbug network information

#### 1.1 network interfaces

show all interfaces (list all interfaces with information: MAC, IP, Operational State, MTU) 

```ip a``` 

only list active (active is not UP operational!) interfaces with information: MAC, IP, MTU, RX/TX Counters)

```ifconfig -a```

show specific interface (ex. eth0)

```ifconfig eth0``` 

show port attributes (ex. eth0), show type of interface, supported link mode, auto-negotiation, speed, link status(OSI layer2, not layer1 physical link)

```ethtool eth0```

show port statistics (ex. eth0)

```ethtool -S eth0```

show optical port attributes (ex. eth0)

```sudo ethtool -m eth0```

check maximum MTU in network segment

```ping -M do -s [MTU-28*] [destination IP]```

* 8 bytes for ICMP header and 20 bytes for Ethernet header

#### 1.2 switching and routing

show global routing table

```route -n```

show next router for a specific destination

```ip route get <destination ip address>```

"traceroute" with maximum MTU between source and destination (not supported by many routers), nice to test LAN/VPN tunnel MTU 

```tracepath <destination ip address>```

show global ARP table

```arp```

show ARP table for specific interface

```arp -i eth0```

show ARP entry for specific host

```arp -a 192.168.1.254```

show ARP table with aditional info (neighbors state)

```ip neigh show```

show IP from MAC 

```ip neighbor | grep "mac-address" | cut -d" " -f1```

### 2. Managing interfaces

Shutdown and shutup interfaces:

In linux you can't phisically (OSI layer1) shutdown an network interface , only in logically (osi layer2)

```ifdown eth0```

Turn up again:

```ifup eth0```

Shutdown and turn up in same command (to not loose remote access):

```ifdown eth0; ifup eth0```

add ip address to interface

```ip addr add 192.168.1.1/24 dev eth0```

remove ip address of interface

```ip addr del 192.168.1.1/24 dev eth0```

### 3. Manage routing

add default gateway

```ip route add default via 192.168.1.254 dev eth0```

add route 

```ip route add 192.168.19.0/24 via 192.168.1.254```

delete route 

```ip route del 192.168.19.0/24 via 192.168.1.254```

### 4. Other

Get public IP address:

```dig +short myip.opendns.com @resolver1.opendns.com```

### 5. IPMI

Restart machine from network

```ipmitool -I lanplus -H <host> -U $IPMIUSER -P $IPMIPASS power reset```

Set IP Address

```ipmitool lan set 1 ipsrc static``` 

```ipmitool lan set 1 ipaddr <ip>```

```ipmitool lan set 1 netmask <mask>```

```ipmitool lan set 1 defgw ipaddr <gw-ip>```

```ipmitool lan set 1 defgw macaddr <gw-mac>```

```ipmitool lan set 1 arp respond on```

Set password

```impitool user set password 2```

Set bootdevice

```ipmitool -I lanplus -H <host> -U $IPMIUSER -P $IPMIPASS chassis bootdev pxe```

```ipmitool -I lanplus -H <host> -U $IPMIUSER -P $IPMIPASS chassis bootdev pxe options=persistent```

Get sensors

```ipmitool -I lanplus -H <host> -U $IPMIUSER -P $IPMIPASS sdr```


  
