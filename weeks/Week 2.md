---
tags: [networking, week-2]
week: 2
---

# Week 2 - Ethernet, MAC Addressing & Switching

## Topics Introduced This Week

- [[Ethernet & MAC Addressing]] - Ethernet standards (802.2 / 802.3), MAC address structure, frame fields, unicast vs broadcast
- [[Data Link Layer]] - Media access control continued: CSMA/CD, CSMA/CA, duplex modes, topologies
- [[Collision Domains & Switching]] - Bridges, switches, MAC address table, frame forwarding methods

## Key Concepts

**Ethernet** is a family of standards (not a single protocol) that collectively define Layer 1 and Layer 2 behaviour. The frame format defined by 802.3 has remained consistent across all speed upgrades from 10 Mb/s to 400 Gb/s.

**CSMA/CD** (wired) and **CSMA/CA** (wireless) are the two media access control mechanisms - how devices share a medium without constantly crashing into each other.

**Switches** replaced hubs by giving each port its own collision domain (micro-segment). The MAC address table is built dynamically by observing source addresses on incoming frames.

## Connections to Later Weeks

- The MAC address table and broadcast domains are central to [[VLANs]] (Week 3)
- Broadcast storms caused by switching loops are solved by [[Spanning Tree Protocol (STP)]] (Week 8)
- The ARP process relies on [[Ethernet & MAC Addressing]] broadcast behaviour - see [[ARP (Address Resolution Protocol)]] (Week 4)
