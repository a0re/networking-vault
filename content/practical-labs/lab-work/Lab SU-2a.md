
# Network Topology:

![[Lab_2a_Topology.png]]


# Addressing Table:

![[Lab_2a_Address_Table.png]]

___ 
## Instructions
#### Part 1: 
- Download Packet Tracer
- Add 2: 3650 Switches, 2 PC's
- Add ***copper cross-over*** & ***copper straight-through*** 
- IP Configuration on PC1 & PC2 setup
- ipconfig & ping on both PC's for testing
#### Part 2:
- Display ARP table 
- Delete ARP table
#### Part 3:
- Change hostname according to the topology to switch 3 & 4 respectively
- add MOTD Switches to Switch 3
- use ``show running-config`` & ``show startup-config``
- use ``copy running-config startup-config`` on Switch 3
- reload Switch 3 & Switch 4
- Adding User Mode Password: ``ccna``
- Adding Administrator mode Password: ``ccna``
- Switch 4: Disable GigabitEthernet Ports 1/0/1 - 4  
- Switch 4: re-enable GigabitEthernet Ports 1/0/1
- Switch 3 & 4: Enter vlan1
- Assign ip address: ``192.168.1.3/4`` subnet mask: ``255.255.255.0``
- Ping between Switch 3 & 4
- write erase to delete configuration
- delete vlan.dat
 
#### Part 4:
- Switch 3 & 4: ``conf t`` 
- create vlan10
- assign gigabitEthernet - 1/0/1 - 1/0/4 to vlan10
- Switch 4: ping 192.168.10.3  
- Switch 3: ping 192.168.10.4 
- Launch Virtual machine launcher 
- Launch PC with Ethernet 
- Launch PC with VAN under Virtual Network
- Configure Internet Protocol (TCP/IP) -> Properties
- PC with Ethernet -> 192.168.1.10
- PC with VAN -> 192.168.1.11
- Ping between PC1 & PC2
- Ping PC1 between Switch 3 & 4
- Ping PC2 between Switch 3 & 4


## Questions

1. PC1 ARP table, what is the MAC address of PC2?

| `Internet Address` | `Physical Address` | `Type`    |
| ------------------ | ------------------ | --------- |
| `192.168.1.11`     | `0010.11bb.d5d0`   | `dynamic` |
2. PC2 ARP table, what is the MAC address of PC1?

| `Internet Address` | `Physical Address` | `Type`    |
| ------------------ | ------------------ | --------- |
| `192.168.1.10`     | `0005.5e9b.6671`   | `dynamic` |
___
#### 1. Is S3 startup configuration empty?
- Yes
#### 2. Is S4 startup configuration empty?
- Yes

#### 3. Is S3 running configuration to default settings?
- No, since banner Switch 3 added ``banner motd`` : 

```
********************  
** This is Week 2 **  
** of Semester 2 **  
** at Swinburne **
********************
```


```
Building configuration...

Current configuration : 1366 bytes

!

version 16.3.2

no service timestamps log datetime msec

no service timestamps debug datetime msec

no service password-encryption

hostname Switch4

no ip cef

no ipv6 cef
```

#### 4. Is S4 running configuration to default settings?
- Yes

```
Building configuration...

Current configuration : 1366 bytes

!

version 16.3.2

no service timestamps log datetime msec

no service timestamps debug datetime msec

no service password-encryption

hostname Switch4

no ip cef

no ipv6 cef
```

___ 
#### Is Switch 3 startup configuration empty?
No, it uses the running configuration

#### Is Switch 4 startup configuration empty?
Yes, no startup configuration exist or saved

#### Is Switch 3 running configuration to default settings?
No, the running config still has the ``banner motd`` & ``hostname`` 

#### Is Switch 4 running configuration to default settings?
Yes, the switch has been reset hostname has been set back to ``Switch``
___ 
#### What’s the status of GigabitEthernet 1/0/1 - 4 interfaces?

| GigabitEthernet       | Status                |
| --------------------- | --------------------- |
| GigabitEthernet 1/0/1 | administratively down |
| GigabitEthernet 1/0/2 | administratively down |
| GigabitEthernet 1/0/3 | administratively down |
| GigabitEthernet 1/0/4 | administratively down |

#### What is the status of GigabitEthernet 1/0/1 - 4 interfaces? 

| GigabitEthernet       | Status |
| --------------------- | ------ |
| GigabitEthernet 1/0/1 | down   |
| GigabitEthernet 1/0/2 | down   |
| GigabitEthernet 1/0/3 | down   |
| GigabitEthernet 1/0/4 | down   |

#### Is this the status output you expected? Yes? No? Why?
Yes & No, I would have expected the gigabitEthernet would **up** instead of down from administratively down.

___ 
#### Will PC1 be able to communicate with PC2? 
No
#### Will PC1 be able to communicate with Switch3 and Switch4?   
Yes
#### Will PC2 be able to communicate with Switch3 and Switch4?
Yes
#### Will the switches be able to communicate with each other? 
No

___
#### Do the existing physical connections match the Topology Diagram on page 1?  
Yes  
#### If not, what discrepancies did you find?  
Yes 

___
#### Can PC1 communicate with PC2? 
Yes
#### Can PC1 communicate with Switch3 and Switch4? 
Yes
#### Can PC2 communicate with Switch3 and Switch4? 
Yes



