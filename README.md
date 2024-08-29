# Campus Architecture Network Documentation

## Packet TracerFile:
The functional packet tracer file that has all of the following technologies implemented in a
The campus architecture network can be found in this git repository.

## Overview:
The goal of this project is to implement the protocols and technologies used to build real-world
enterprise networks using the campus network architecture. This project was completed to build
a strong foundation in the core concepts of switching technologies and routing protocols, while
also developing practical skills and critical thinking abilities that can be applied in real-world
network environments.

## Protocols and Technologies Implemented:

## VLANs:

A VLAN is a network construct that allows network administrators to partition and isolate a
single physical network into multiple distinct broadcast domains.

The Network has 10 VLANs:

| VLAN ID | Name | IPv4 Address |
|----------|----------|----------|
| 1    | Default   | 10.1.0.0/16   |
| 10    | Admin   | 10.10.0.0/16  |
| 20    | HR   | 10.20.0.0/16   |
| 30    | Sales   | 10.30.0.0/16   |
| 40    | Engineering   | 10.40.0.0/16   |
| 90    | Voice   | 10.90.0.0/16   |
| 100    | Guest   | 10.100.0.0/16   |
| 180    | Management   | 10.180.0.0/16   |
| 254    | Native   | 10.254.0.0/16   |
| 255    | Parking-Lot   | 10.255.0.0/16   |


Each VLAN is associated with a unique IP network so the devices within the same VLAN
communicate with each other using IP addressing even if they are spread across different
physical locations.


### Trunking:

A trunk is a network link designed to carry multiple VLANs through a single interface by tagging
frames with VLAN identification. This allows for efficient VLAN distribution across switches and
network segments, enabling devices in different VLANs to communicate through the same
physical link.

### Spanning Tree Protocol(STP):

STP is a network protocol designed to prevent loops in a Layer 2 network. It achieves this by
selectively blocking certain paths and allowing only one active path between switches. This
ensures that there is a single, loop-free path for data traffic, thus preventing broadcast storms
and ensuring network stability.

### EtherChannel:

EtherChannel is a port link aggregation technology or protocol developed by Cisco. It allows the
bundling of several physical Ethernet links to form a single logical link between two networking
devices. This enhances the bandwidth by combining multiple links’ capacities and provides
redundancy for higher network resilience.

### First Hop Redundancy Protocol(FHRP):

FHRP is a group of protocols used to ensure network availability by automatically forwarding
traffic to a standby router or Layer 3 switch if the primary default gateway fails. FHRP with
protocols like HSRP, allows for the configuration of a virtual router with a virtual IP and MAC
address that network hosts use as their default gateway.

### DHCPv4 and DHCPv4 Relay:

Dynamic Host Configuration Protocol version 4 (DHCPv4) is a network protocol used to
automatically assign IP addresses and other network configuration parameters to devices on a
network. It enables devices to request and receive an IP address, default gateway, and subnet
mask automatically from a DHCP server. This eliminates the need for manual network
configuration.

DHCPv4 Relay allows DHCP clients on different subnets/VLANs to communicate with a
centralized DHCP server. When a client sends a request, the relay agent intercepts it and
forwards the request to the DHCP server, adding the necessary information about the client’s
subnet, which enables the server to assign an appropriate IP address to the client.


### Layer 3 Switching:

Layer 3 switching is a process where a switch uses IP addresses to make forwarding decisions.
This effectively combines the functions of a switch and a router, which allows for high-speed
touting of data within a network. This approach allows for inter-VLAN routing, as it lets devices
on different VLANs communicate without the need for a separate router.

### SwitchVirtualInterfaces(SVIs):

An SVI is a virtual interface with an IP address that is associated with a specific VLAN. An SVI
on a Layer 3 switch enables it to perform routing functions between VLANs and IP networks,
without the need for an external router. The SVI on a Layer 3 switch is typically the default
gateway for end devices on the same VLAN.

### ManagementVLAN:

This network has a management VLAN for these reasons:

**Security:** VLAN 1 is the default VLAN on many switches, and it carries all untagged traffic.  
Because of this, it's often targeted for VLAN hopping and other network attacks. By moving important services like management interfaces (SVI) to another VLAN, you reduce the risk of these services being compromised.

**Traffic Segmentation:** Using a separate VLAN for management traffic ensures that it is isolated from user data traffic, reducing the risk of congestion and performance issues on the management interface. This segmentation also helps in prioritizing traffic, ensuring that management traffic is always accessible, even during high data traffic periods.

**Best Practice and Compliance:** Many network security standards and best practices recommend segregating management and user data traffic. This segregation helps in adhering to compliance requirements for network security.

**Network Organization:** Having a dedicated VLAN for management purposes helps in organizing the network more efficiently. It allows network administrators to apply specific policies and controls to the management traffic without affecting the user data traffic.


### OpenShortestPathFirst(OSPF):

OSPF is a robust, link-state routing protocol widely used in IP networks. OSPF efficiently
calculates the shortest path to each network destination by constructing a complete map or
topology of the network using link-state advertisements (LSAs) from all participating routers. As
an interior Gateway Protocol, OSPF is predominantly utilized within a single routing domain or
autonomous system.

## Example Configurations:

### Access Switch (with DHCP Server) Interface:

**!**

**hostname access-1-1**

**!**

**no ip domain-lookup**

**!**

**enable secret class**

**!**

**line con 0**

**logging synchronous**

**exec-timeout 0 0**

**exit**

**!**

**line vty 0 4**

**password cisco**

**login**

**transport input telnet**

**exit**

**!**

**!**

**vlan 10**

**name admin**

**exit**

**!**

**vlan 20**

**name HR**

**exit**

**!**

**vlan 30**

**name Sales**

**exit**

**!**

**vlan 40**

**name Engineering**

**exit**

**!**

**vlan 90**

**name Voice**

**exit**

**!**

![ref1]

<a name="br5"></a> 

**vlan 100**

**name Guest**

**exit**

**!**

**vlan 180**

**name Management**

**exit**

**!**

**vlan 254**

**name Native**

**exit**

**!**

**vlan 255**

**name Parking-Lot**

**exit**

**!**

**!**

**interface range fa0/1-24, g0/1-2**

**switchport mode access**

**switchport access vlan 255**

**shutdown**

**exit**

**!**

**!**

**interface fa0/10**

**switchport mode access**

**switchport access vlan 10**

**no shutdown**

**exit**

**!**

**interface fa0/20**

**switchport mode access**

**switchport access vlan 20**

**no before shutdown**

**exit**

**!**

**!**

**interface g 0/1**

**no switchport access vlan 255**

**switchport mode trunk**

**switchport nonegotiate**

**switchport trunk allowed vlan 1,10,20,30,40,90,100,180,254**

**switchport trunk native vlan 254**

**no shutdown**

![ref1]

<a name="br6"></a> 

**exit**

**!**

**!**

**interface vlan 180**

**ip address 10.180.1.10 255.255.255.0**

**no shutdown**

**exit**

**!**

**ip default-gateway 10.180.1.1**

**!**

**!**

**port-channel load-balance src-dst-ip**

**!**

**interface range fa 0/1-2**

**shutdown**

**channel-protocol lacp**

**channel-group 1 mode active**

**exit**

**!**

**!**

**interface range fa 0/1-2**

**no shutdown**

**exit**

**!**

**!** The following port-channel1 command is only required for Packet Tracer

**interface port-channel1**

**switchport trunk allowed vlan 1,10,20,30,40,90,100,180,254**

**exit !**

**!**

**interface g 0/1**

**no switchport access vlan 255**

**switchport mode access**

**switchport access vlan 40**

**no shutdown**

**exit**

**!**

**!**

**interface fa 0/21**

**no switchport access vlan 255**

**switchport mode access**

**switchport access vlan 40**

**no shutdown**

**exit**

**!**

**!**

**interface fa 0/21**

**no switchport access vlan 255**

**switchport mode access**

**switchport access vlan 40**

**spanning-tree portfast**

**spanning-tree bpduguard enable**

**no shutdown**

**exit**

**!**

**!**

**interface fa 0/21**

**no switchport access vlan 40**

**switchport mode access**

**switchport access vlan 255**

**shutdown**

**exit**

**!**

**!**

**interface GigabitEthernet0/1**

**switchport trunk native vlan 254**

**switchport trunk allowed vlan 1,10,20,30,40,90,100,180,254**

**switchport mode trunk**

**switchport nonegotiate**

**exit**

**!**

**!**

**interface gig 0/2**

**no switchport access vlan 255**

**switchport trunk native vlan 254**

**switchport trunk allowed vlan 1,10,20,30,40,90,100,180,254**

**switchport mode trunk**

**switchport nonegotiate**

**no shutdown**

**exit**

**!**


### Distribution Switch:

**!**

**hostname distribution-1**

**!**

**no ip domain-lookup**

**!**

**enable secret class**

**!**

**line con 0**

**logging synchronous**

**exec-timeout 0 0**

**exit**

**!**

**line vty 0 4**

**password cisco**

**login**

**transport input telnet**

**exit**

**!**

**!**

**vlan 10**

**name admin**

**exit**

**!**

**vlan 20**

**name HR**

**exit**

**!**

**vlan 30**

**name Sales**

**exit**

**!**

**vlan 40**

**name Engineering**

**exit**

**!**

**vlan 90**

**name Voice**

**exit**

**!**

**vlan 100**

**name Guest**

**exit**

**!**

**vlan 180**

**name Management**

**exit**

**!**

**vlan 254**

**name Native**

**exit**

**!**

**vlan 255**

**name Parking-Lot**

**exit**

**!**

**!**

**interface range g 1/0/1-24, g 1/1/1-4**

**switchport mode access**

**switchport access vlan 255**

**shutdown**

**exit**

**!**

**interface range g 1/0/1-2**

**no switchport access vlan 255**

**switchport mode trunk**

**switchport nonegotiate**

**switchport trunk allowed vlan 1,10,20,30,40,90,100,180,254**

**switchport trunk native vlan 254**

**no shutdown**

**exit**

**!**

**!**

**interface vlan 10**

**ip address 10.10.0.1 255.255.0.0**

**no shutdown**

**exit**

**!**

**interface vlan 20**

**ip address 10.20.0.1 255.255.0.0**

**no shutdown**

**exit**

**!**

**!**

**interface vlan 10**

**ip address 10.10.0.1 255.255.0.0**

**no shutdown**

**exit**

**!**

**interface vlan 20**

**ip address 10.20.0.1 255.255.0.0**

**no shutdown**

**exit**

**!**

**interface vlan 180**

**ip address 10.180.1.1 255.255.255.0**

**no shutdown**

**exit**

**!**

**!**

**interface g 1/0/24**

**no ip address**

**exit**

**!**

**interface Port-channel2**

**no switchport**

**ip address 10.111.1.1 255.255.255.0**

**no shutdown**

**exit**

**!**

**interface range gig 1/0/23-24**

**no switchport**

**channel-protocol lacp**

**channel-group 2 mode active**

**no shutdown**

**exit**

**!**

**end**

**!**

**!**

**interface vlan 10**

**ip helper-address 10.40.0.99**

**no shutdown**

**exit**

**!**

**interface vlan 20**

**ip helper-address 10.40.0.99**

**no shutdown**

**exit**

**!**

**!**

**interface vlan 10**

**no ip address**

**ip address 10.10.0.5 255.255.0.0**

**no shutdown**

**exit**

**!**

**!**

**interface vlan 20**

**no ip address**

**ip address 10.20.0.5 255.255.0.0**

**no shutdown**

**exit**

**!**

**!**

**interface vlan 30**

**ip address 10.30.0.5 255.255.0.0**

**no shutdown**

**exit**

**!**

**interface vlan 40**

**ip address 10.40.0.5 255.255.0.0**

**no shutdown**

**exit**

**!**

**!**

**interface range gig 1/0/3 - 4**

**shutdown**

**no switchport access vlan 255**

**switchport trunk native vlan 254**

**switchport trunk allowed vlan 1,10,20,30,40,90,100,180,254**

**switchport mode trunk**

**switchport nonegotiate**

**no shutdown**

**exit**

**!**

**!**

**interface vlan 10**

**standby 1 ip 10.10.0.1**

**standby 1 priority 200**

**standby 1 preempt**

**no shutdown**

**exit**

**!**

**interface vlan 20**

**standby 1 ip 10.20.0.1**

**standby 1 priority 200**

**standby 1 preempt**

**no shutdown**

**exit**

**!**

**interface vlan 30**

**standby 1 ip 10.30.0.1**

**standby 1 priority 200**

**standby 1 preempt**

**no shutdown**

**exit**

**!**

**interface vlan 40**

**standby 1 ip 10.40.0.1**

**standby 1 priority 200**

**standby 1 preempt**

**no shutdown**

**exit**

**!**

**!**

**spanning-tree vlan 10 root primary**

**spanning-tree vlan 20 root primary**

**!**

**spanning-tree vlan 30 root secondary**

**spanning-tree vlan 40 root secondary**

**!**

**!**

**router ospf 1**

**router-id 5.5.5.5**

**log-adjacency-changes**

**redistribute connected**

**network 10.10.0.0 0.0.0.255 area 0**

**network 10.20.0.0 0.0.0.255 area 0**

**network 10.111.1.0 0.0.0.255 area 0**

**network 10.180.1.0 0.0.0.255 area 0**

**!**

### Core Switch:

**!**

**no ip domain-lookup**

**!**

**line con 0**

**logging synchronous**

**exec-timeout 0 0**

**exit**

**!**

**!**

**interface g1/0/1**

**no switchport**

**ip address 10.200.200.1 255.255.255.252**

**exit**

**!**

**interface g1/0/3**

**no switchport**

**ip address 10.200.200.9 255.255.255.252**

**exit**

**!**

**interface g1/0/6**

**no switchport**

**ip address 10.200.200.17 255.255.255.252**

**exit**

**!**

**interface g1/0/24**

**no switchport**

**ip address 10.1.1.1 255.255.255.0**

**exit**

**!**

**interface g1/0/23**

**no switchport**

**ip address 10.200.200.26 255.255.255.252**

**exit**

**!**

**!**

**router ospf 1**

**router-id 1.1.1.1**

**network 10.1.1.0 0.0.0.255 area 0**

**network 10.200.200.24 0.0.0.3 area 0**

**network 10.200.200.8 0.0.0.3 area 0**

**network 10.200.200.16 0.0.0.3 area 0**

**network 10.200.200.0 0.0.0.3 area 0**

**end**

**!**

**clear ip ospf process**

**Reset ALL OSPF processes? [no]: yes**

**!**

**!**

**interface g 1/0/3**

**ip ospf hello-interval 5**

**ip ospf dead-interval 20**

**exit**

**!**

**!**

**interface gig 1/0/1**

**ip ospf network point-to-point**

**exit**

**!**

**interface gig 1/0/3**

**ip ospf network point-to-point**

**exit**

**!**

**interface gig 1/0/6**

**ip ospf network point-to-point**

**exit**

**!**

**interface gig 1/0/23**

**ip ospf network point-to-point**

**exit**

**!**

**!**

**router ospf 1**

**passive-interface gig 1/0/24**

**end**

**!**

### Edge Router:

**!**

**interface GigabitEthernet0/0/0**

**ip address 10.200.200.25 255.255.255.252**

**no shutdown**

**exit**

**!**

**interface GigabitEthernet0/0/1**

**ip address 209.1.1.1 255.255.255.252**

**no shutdown**

**exit**

**!**

**!**

**router ospf 1**

**router-id 10.10.10.10**

**network 10.200.200.24 0.0.0.3 area 0**

**end**

**!**

**clear ip ospf process**

**Reset ALL OSPF processes? [no]: yes**

**!**

**!**

**ip route 0.0.0.0 0.0.0.0 209.1.1.2**

**end**

**!**

**!**

**router ospf 1**

**default-information originate**

**end**

**!**

**!**

**ip route 10.0.0.0 255.0.0.0 null0**

**end**

**!**

**!**

**router ospf 1**

**auto-cost reference-bandwidth 10000**

**end**

**!**

**!**

**interface gig 0/0/0**

**ip ospf network point-to-point**

**exit**

**!**

### ISP Router:

**!**

**interface GigabitEthernet0/0/1**

**ip address 209.1.1.2 255.255.255.252**

**no shutdown**

**exit**

**!**

**interface Loopback0**

**ip address 99.99.99.99 255.255.255.0**

**no shutdown**

**exit**

**!**

**!**

**ip route 10.0.0.0 255.0.0.0 209.1.1.1**

**end**

**!**
