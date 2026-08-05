
# Network Topology: 

![[Lab_3a_Topology.png]]

# Addressing Table:

![[Lab_3a_Addressing_Table.png]]

# VLAN Table:
![[Lab_3a_VLAN_Table.png]]


## Instructions: 
1. Power on Switch 3 & 4
2. ``show ip interface brief``
3. Switch 3 & 4 ``ena`` -> ``show startup-config`` 
4. Rename switch 3 & switch 4 hostname - ``conf t`` -> ``hostname [name]
5. add banner -> ``banner motd [d] [message] [d]``
6. ``no ip domain-lookup``
7. ``line console 0``
8. ``logging synchronous``
9. Switch 3 & Switch 4 -> ``show vlan brief``
10. Switch 3 & 4: ``conf t`` -> ``vlan 10`` -> ``name Users`` -> ``end``
11. Switch 3 & 4: ``show vlan brief``
12. Configure Switch 3 gigabitEthernet 10-14 to vlan 10
	- ``conf t`` -> ``interface range gigabitEthernet 1/0/10 - 14`` -> ``switchport mode access`` -> ``switchport access vlan 10`` -> ``show vlan brief``
13. Configure Switch 4 gigabitEthernet 10-13 to vlan 10 
	- ``conf t`` -> ``interface range gigabitEthernet 1/0/10 - 13`` -> ``switchport mode access`` -> ``switchport access vlan 10`` -> ``show vlan brief``
14. Plug in Ethernet Cable [1] to Switch 3 1/0/7
15. Open Virtual Machine Launcher -> ``Launch PC with Ethernet`` -> configure ip: 192.169.1.3 & 255.255.255.0 
17. Open Virtual Machine Launcher -> ``Launch PC with VAN`` -> configure ip: 192.169.1.4 & 255.255.255.0
18. ip config Ethernet & VLAN
19. ``getmac`` to get MAC Address - Ethernet & VLAN
21. Disable VLAN 
	- ``conf t`` -> ``interface VLAN 1`` -> shutdown 
22. Create VLAN 99 -> Name Management -> Exit
23. Interface VLAN 99 -> description Management -> ip address 192.168.99.3 255.255.255.0 -> exit
24. ``show vlan brief`` & ``show ip interface brief
25. 

___


#### How many VLANs are there in Cisco Switch by default

5 VLANs: 
	- [1] default
	- [1002] fddi-default
	- [1003] token-ring-default
	- [1004] fddinet-default
	- [1005] trnet-default

#### What is default VLAN membership of interfaces Gi1/0/1 - 24? 

[1] default 

#### Test Your Network
a. PC1 Communication with PC2
	Yes
b. MAC Address of PC1
	``Physical Address`` -> ``00-0C-29-CE-6B-F8``
c. MAC Address of PC2
	``Physical Address`` -> ``00-0C-29-4F-F5-FE``

#### Connectivity Scenarios
- PC1 with IP 192.168.1.3/24 connects to Gi1/0/7 on Switch3  
- PC2 with IP 192.168.1.4/24 connects to Gi1/0/24 on Switch4  
- PC3 with IP 192.168.10.10/24 connects to Gi1/0/10 on Switch3  
- PC4 with IP 192.168.10.11/24 connects to Gi1/0/11 on Switch3  
- PC5 with IP 192.168.10.7/24 connects to Gi1/0/13 on Switch 4


#### Can Switch3 Ping Switch 4?
timeout 2 seconds

#### ``show ip ssh``
What is the default SSH Version?
SSH Enabled - Version 1.99

What is the default session timeout?
Authentication time: 120secs; 

How many authentication retries by default?
Authentication retries: 3;

Which are the unused ports on Switch3?
Gi 1/0/1 - Gi 1/1/4

Which are the unused ports on Switch4?
Gi 1/0/1 - Gi 1/1/4

Configure all VLAN 10 ports on Switch 3 with the following setting:
	• Maximum MAC address allowance: 3  
	• Violation action: shutdown (this is the default setting)  
	• Sticky MAC address learning

a) Configure VLAN 10 port on Switch 4 with the following port-security settings:  
• Maximum MAC address allowance: 1  
• Violation action: restrict  
• Only allow MAC address: 0060.5cd2.9b05  
Because port Gi1/0/24 on Switch 4 connects to the VAN PC, there are currently two connected MAC  
addresses to it, i.e. the VAN system server’s MAC, and the VAN PC’s MAC. Also, the connected MAC  
addresses do not match the statically configured one (i.e. 0060.5cd2.9b05). Therefore, the port-security policy  
has been breached and the port should now be in err-disabled mode.  
What command can you use to validate that port Gi1/0/24 is in err-disabled mode? ____________________
What steps should you take to re-enable this port?   

same network same vlan = intra-vlan

