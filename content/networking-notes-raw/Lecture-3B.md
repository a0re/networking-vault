# Best Practices

- Switch ports are enabled by default.
- Access ports are prepatched to outlets in public / semi-public spaces. If any user plugs a computer, they get access to that VLAN.

## Types of Attacks in Layer 2

### MAC Address Flooding

- Since new entries are added to a table, under this attack the switch will fill up a CAM table with multiple MAC addresses.
  - Due to the limited space the MAC address table has.
  - The consequences are the switches are constantly flooding the frame and ports due to being unable to find the correct MAC address.

### MAC Spoofing Attack

- An attacking computer changing its source MAC address to a legitimate host in the network.
- If the attacker changes the MAC address of the default gateway of a VLAN, it will redirect traffic towards the attacker.
- Issues - Traffic will never reach intended destination.
- Attackers can also sniff the packet and obtain the information.
- Further security if unintentional enabled ports - further precaution.

## Blackhole VLANs

- Custom VLANs that isolates unused ports and never carry user and management traffic.
- Move ports to an isolated VLAN.
- Shutdown ports.

## Management VLAN

- By default VLAN 1 and carries layer 2 control traffic on remote management configuration and session traffic.
- Allows the view of the logical topology and configuration to the attacker.

## Force Switchport Mode

- By default DTP is dynamic auto and will establish a trunk connection when requested.

### Attackers

- An attack can spoof or act as a switch and establish a trunk in the network.
- Trunk port has access to all VLANs.

## Best Practice

- All unused ports and ports connected is forced to be in access mode.
- Only ports meant to be trunk should be forced into trunk mode.
- Access + unused ports - mode access.

***By default, port security is disabled.***

## Port Security

- Allow the limit of MAC addresses are allowed on a port.
- Only traffic from legitimate MAC addresses.

### Configuring MAC Addresses

- **Static:** Programmed MAC addresses.
- **Dynamic:** Current MAC addresses.
- **Sticky:** Switch auto-adds MAC address up to limit.

### Action When Violated

- **Protect:** Invalid frames are dropped, valid frames are sent.
- **Restrict:** Protect but violation counter is incremented.
- **Shutdown:** Port goes to error-disabled state.

### Default Port Security

- **Port Security:** Disabled.
- **Max MAC Address:** 1.
- **Violation Mode:** Shutdown.
- **Sticky Address Learning:** Disabled.