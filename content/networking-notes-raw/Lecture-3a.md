# VLANs

- It is sometimes more convenient to break down layer 2 segments into separate broadcast domains.
- If multiple switches are connected to each other, it can be configured to exist across multiple switches.
- VLANs exist in different switches but same VLAN allows for the same broadcast domain.
- PCs aren't aware of their connection / VLAN and think of being connected to the same switch.
- VLANs -4095 can't be created and reserved for intended use / network purposes.

## Access Ports

- Only carry traffic of one VLAN.
- Frame entering a switchport is untagged -due to PCs inability to know the VLAN configuration.
- When the frame is entered through the switch, it will insert the VLAN ID.
- VLAN IDs are removed when the frame goes outward to a PC.

**TL;DR:** When entering the switch it is untagged. When leaving the switch it is untagged.

→ When setting ports to access they belong to VLAN?

Traffic is restricted based on: Traffic for that VLAN / Contents of CAM table / MAC table.

## VLAN Membership

**Static VLAN** -Ports are manually assigned to VLAN, e.g., `switchport access vlan`. Reconfiguration if happens.

**Dynamic VLAN** - Membership is configured via VMPS - VLAN Membership Policy Server, based on source MAC address.

**VLAN Trunks** - Able to carry traffic for multiple VLANs and sent with a VLAN ID tag in the frame.

- It partitions on the physical link into multiple logical links that carries traffic per 1 VLAN.
- Frames are tagged when ports are trunked to know which VLAN traffic it belongs to.
- Traffic in trunk ports are segmented.
- On layer 3 device, 1 physical port can have multiple logical interfaces for each VLAN.

**Performance:**

- Reduce broadcast domains, traffic and frames.
- Forcing groups to traverse through layer 3 devices -restrict traffic at higher.
- Security.
- Cost reduction.
- Better performance -shrinking broadcast domain.
- Shrinking broadcast domains -unnecessary traffic, i.e., ARP request.
- Easier to administrate.

## Intra VLAN Communication

- Layer 2 communication - if host of the same VLAN communicates with another host.
- Switch and router will get the ARP request too.

[Diagram]

## VLAN Ranges

- VLANs are 12 bits long in the Ethernet header -802.1Q specification.
- Meaning 2^12 unique VLANs - 0 to 4095 (starts from 0).

## Tagging

- Inserting VLAN ID to the Ethernet frame so the switch can identify which VLAN the frame belongs to.

VLANs are split into two categories:

- **Normal Range VLANs:** 1-1005 (Stored in flash memory).
- **Extended Range VLANs:** 1006-4095 (Saved in the running config).

- VLAN is a logical partition of the layer 2 network.
- Multiple VLANs can be created.
- The VLANs are isolated and packets can only pass between them via layer 3.
- VLANs allow the division of the physical switch into virtual number of switches.

**Benefits:**

- Security and performance: Segregating switches reduce the broadcast domain increasing communication performance with fewer domains.
- Computers within different work groups with the use of VLANs can restrict traffic that restrict the amount and type between 2 groups.
- E.g., Layer 2 attack effects only 1 group but not another.

- Traffic and communication between different layer 2 segments require a layer 3 device -(router).
- Routers prevent the broadcast from going from 1 network to another and limit the type of traffic of a network.

## Before VLANs

- To segregate traffic, different physical switches / hops per each work group, meaning close proximity to the physical location work station.
- 1 router per work group and it doesn't scale.
- Inefficient traffic flow between in the same work - requiring a layer 3 device to communicate.
- Resulting in multiple hubs per port to segregate the work groups.

- VLANs allow for independent logical area network even when sharing switch and infrastructure.
- Cost effectiveness lies within VLANs being aggregated to a single physical port to a layer 3 device.

# VLAN Tagging

Two major methods of frame tagging:

- **Cisco Proprietary** - Inter-Switch Link (ISL).
- **IEEE** -802.1Q.

1. VLAN identifiers are added when entering the switch.
2. Frames are then forwarded appropriately switches based on VLAN and MAC address.

## Tagging Ethernet

[ETHERNET FRAME]

[802.1Q Frame]

- The included tag field is a 4-byte long field -Last 12 bits are used to mark the VLAN identifier / tagging.

## Native VLANs

- Frames that belong to native VLANs are not tagged.
- Special VLANs that aren't carried on trunk ports.
- Frames that are untagged when received remain untagged are placed in native VLAN when forwarded.
- If no ports are part of the native VLAN and no other trunk links, the frame is dropped.

## Dynamic Trunk Protocol

- Trunks that negotiation with each other are Dynamic Trunking Protocol (DTP).
- By default DTP configuration is set to dynamic auto.

## Negotiated Interfaces Model

- When setting the port to dynamic desirable with other device, a trunk.
- **Dynamic Auto** -The port will listen to request to be a trunk but never initiate.
- **Forceably set a port to be a trunk port.**
- **Forceably set a port to be an access port.**

- **Nonegotiate** disables DTP and will not send or receive any negotiation.
- If both ends are set to dynamic auto, the outcome would be setting the port to access link. No one will initiate for being a trunk.

# Types of VLANs

## Data VLANs

- Configured to carry data traffic.
- Any created VLANs that are used by network devices.
- Separates user traffic.

## Default VLANs

- Default settings on unconfigured switches.
- For Cisco - VLAN 1.
- Cannot be deleted or renamed.
- Carries all layer 2 traffic.

## Native VLANs

- Backwards compatible with switches.
- Non-tagged frames when Ethernet trunk.
- For Cisco - VLAN 1.
- Used to tag trunk links and carry untagged traffic.

## Management VLANs

- Assign with IP address to network layer connectivity.
- Default VLAN 1.
- VLAN used to manage and access networks / administrators.

## Voice VLANs

- Used to handle voice traffic.