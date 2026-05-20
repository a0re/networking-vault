---
tags: [networking, vlan, routing, layer-3, week-7]
week: 7
aliases: [Inter-VLAN, Router-on-a-Stick, ROAS, SVI, Sub-interface]
---

# Inter-VLAN Routing

> **Source:** [[Week 7]]

## The Problem

[[VLANs]] create isolated Layer 2 broadcast domains. A host in VLAN 10 **cannot communicate** with a host in VLAN 20 at Layer 2 - the switch will not forward frames between VLANs.

To route between VLANs, traffic must pass through a **Layer 3 device** (a router or a Layer 3 switch).

---

## Important Clarifications

- **Switches** store MAC addresses per port - they do **not** maintain ARP tables
- Only **routers and hosts** initiate ARP requests
- A host sends frames out untagged; the **switch adds** the VLAN tag when forwarding on a trunk
- When a packet re-enters the switch from a router (destined for a different VLAN), **the switch retaggs the frame** with the correct VLAN ID

---

## Method 1: Router-on-a-Stick (ROAS)

A single router port is connected to the switch via a **trunk link**. The router port is divided into **sub-interfaces**, each associated with a specific VLAN.

```
Router
├── Gig0/0.10 → VLAN 10 (192.168.10.1/24)
├── Gig0/0.20 → VLAN 20 (192.168.20.1/24)
└── Gig0/0.30 → VLAN 30 (192.168.30.1/24)
     ↓
   Trunk link (802.1q)
     ↓
   Switch
```

### How a ROAS Packet Travels

1. Host in VLAN 10 sends an untagged frame to its default gateway IP
2. Switch receives the frame on an access port, tags it with VLAN 10, forwards up the trunk to the router
3. Router sub-interface Gig0/0.10 strips the VLAN 10 tag, decapsulates the frame, reads the destination IP
4. Router consults routing table → destination is in VLAN 20 subnet
5. Router re-encapsulates the packet with a new frame, tags it with VLAN 20 via Gig0/0.20
6. Frame travels down the trunk to the switch
7. Switch receives the frame tagged VLAN 20, strips the tag, forwards out the appropriate access port to the destination host

### Benefits
- Cost-effective - only one physical cable and one router port
- Simpler cabling
- Enables VLAN segregation while maintaining inter-VLAN communication

### Limitation
- **Bandwidth contention** - all inter-VLAN traffic shares one physical link

---

## Method 2: Layer 3 Switch with SVIs

A **Layer 3 switch** can route natively using **Switch Virtual Interfaces (SVIs)** - logical interfaces associated with each VLAN that act like router interfaces.

```
Layer 3 Switch
├── SVI VLAN 10 → IP 192.168.10.1/24
└── SVI VLAN 20 → IP 192.168.20.1/24
```

- No external router needed
- Routing happens in hardware - much faster than ROAS
- Good practice: assign VLAN IDs that match subnet addresses for clarity (e.g. VLAN 10 → 192.168.**10**.x)

---

## Default Gateway Configuration

Each host needs three things configured:
- **IP address** - unique within the VLAN subnet
- **Subnet mask** - defines the network boundaries
- **Default gateway** - the router sub-interface IP (or SVI IP) for that VLAN

---

## Related Notes

- [[VLANs]] - VLAN concepts, 802.1q tagging, trunk links
- [[IPv4 Routing]]  how routing tables and packet forwarding work
- [[ARP (Address Resolution Protocol)]] - ARP within each VLAN
- [[Data Encapsulation]] - encapsulation/decapsulation at the router
