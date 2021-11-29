# 配置物理机和虚拟机连通性
`在物理机没有外网的情况下或者更换网络的时候，仍可以通过固定ip进行虚拟机`

[配置教程](https://blog.csdn.net/byc6352/article/details/92806380)

`sudo vi /etc/networks/interfaces`
```
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.0.0.3
netmask 255.255.255.0
gateway 10.0.0.2
```

`sudo vi /etc/resolv.conf`
```
domain
nameserver 114.114.114.114
nameserver 8.8.8.8
search localdomain
```

`sudo vi /Library/Preferences/VMware\ Fusion/networking`
```
VERSION=1,0
answer VNET_1_DHCP yes
answer VNET_1_DHCP_CFG_HASH BCFD3EB68EB3CAA3333988A779F361254990FB70
answer VNET_1_HOSTONLY_NETMASK 255.255.255.0
answer VNET_1_HOSTONLY_SUBNET 172.16.212.0
answer VNET_1_HOSTONLY_UUID 2A2E39DB-BCD1-4AFB-919E-6DDB2E5E3617
answer VNET_1_VIRTUAL_ADAPTER yes
answer VNET_3_DHCP yes
answer VNET_3_HOSTONLY_NETMASK 255.255.255.0
answer VNET_3_HOSTONLY_SUBNET 10.0.0.0
answer VNET_3_HOSTONLY_UUID 611B65AF-0AE7-4EFD-9572-0BEF47E03E0D
answer VNET_3_NAT yes
answer VNET_3_NAT_PARAM_UDP_TIMEOUT 30
answer VNET_3_VIRTUAL_ADAPTER yes
answer VNET_8_DHCP yes
answer VNET_8_DHCP_CFG_HASH BB09D613951A3D442EAC9F3994EB5FACAF9435FE
answer VNET_8_HOSTONLY_NETMASK 255.255.255.0
answer VNET_8_HOSTONLY_SUBNET 192.168.56.0
answer VNET_8_HOSTONLY_UUID BBEF474C-61F4-4D46-9D35-F67DBC4A2ED6
answer VNET_8_NAT yes
answer VNET_8_VIRTUAL_ADAPTER yes
add_bridge_mapping en0 2
answer VNET_3_DHCP_CFG_HASH 2FFD6EC40A3D37ABDDAF2FDC955AA92A2110CDB7
```

`sudo vim /Library/Preferences/VMware\ Fusion/vmnet3/nat.conf`
```
# VMware NAT configuration file
# Manual editing of this file is not recommended. Using UI is preferred.

[host]

# Use MacOS network virtualization API
useMacosVmnetVirtApi = 1

# NAT gateway address
ip = 10.0.0.2
netmask = 255.255.255.0

# VMnet device if not specified on command line
device = vmnet3

# Allow PORT/EPRT FTP commands (they need incoming TCP stream ...)
activeFTP = 1

# Allows the source to have any OUI.  Turn this on if you change the OUI
# in the MAC address of your virtual machines.
allowAnyOUI = 1

# Controls if (TCP) connections should be reset when the adapter they are
# bound to goes down
resetConnectionOnLinkDown = 1

# Controls if (TCP) connection should be reset when guest packet's destination
# is NAT's IP address
resetConnectionOnDestLocalHost = 1

# Controls if enable nat ipv6
natIp6Enable = 0

# Controls if enable nat ipv6
natIp6Prefix = fd15:4ba5:5a2b:1003::/64

[tcp]

# Value of timeout in TCP TIME_WAIT state, in seconds
timeWaitTimeout = 30

[udp]

# Timeout in seconds. Dynamically-created UDP mappings will purged if
# idle for this duration of time 0 = no timeout, default = 60; real
# value might be up to 100% longer
timeout = 30

[netbios]
# Timeout for NBNS queries.
nbnsTimeout = 2

# Number of retries for each NBNS query.
nbnsRetries = 3

# Timeout for NBDS queries.
nbdsTimeout = 3

[incomingtcp]

# Use these with care - anyone can enter into your VM through these...
# The format and example are as follows:
#<external port number> = <VM's IP address>:<VM's port number>
#8080 = 172.16.3.128:80

[incomingudp]

# UDP port forwarding example
#6000 = 172.16.3.0:6001
```
