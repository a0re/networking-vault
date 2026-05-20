# Inter-VLAN Routing

- Routing needs to go down the communication stack from one device to another by encapsulating and decapsulating packets through each device until it reaches the destination.

- Point-to-point serial link - Layer 2 addressing doesn't exist.

- **L** - Routes represents local route to match the IP address configured.
- **C** - Routes connected directly to the network.

## Default Gateways

- **IP address** - Unique host on local network.
- **Subnet mask** - Host's network subnet.
- **Default gateway** - Identify the router and packet sent for outside communication.

## Routing / ARP

- Needed when device knows IP but needs MAC.
- Switches stored MAC address on port only.
- Don't maintain ARP tables.
- Don't store ARP info.

- Only router and host requests ARP.

## Tagging

- Sending untagged from host.
- Switch adds a tag if it travels across a link.
- Router redirects with trunk or tag.
- Only when the packet enters the switch does it retag to different VLAN.

- Layer 2 cannot forward traffic between VLANs without the assistance of routers.
- Inter-VLAN only forwards traffic from one to another.

## Routers

- Sub interfaces divides 1 physical to many logical interfaces.
- Each interface has 65,535 logical interfaces (2^16).

**Sub Interfaces:**

- Bandwidth contention?
- Less expensive.
- 1 to many physical interface.
- Connect to trunk mode switchports.
- More complex connection config.

- **Layer 3 Switch Routing** -To retag the frame, switches uses (SVI) switch virtual interface. Acting like routing interfaces.

- Good practice -Assign VLAN number the same as subnet addresses.

## Router-on-a-Stick (ROAS)

- Without ROAS, VLANs are isolated to Layer 2.

### Benefits

- Cost-effective.
- Simpler cabling.
- Easier management.
- Allow VLAN segregation while still having inter-VLAN communication.

### Example

Steps:

1. 802.1Q frame.
2. Re-encapsulates.
3. Determine exit port -(route table + dest. IP).
4. Re-encapsulates with new source and destination MAC and change VLAN tag?
5. [Missing]