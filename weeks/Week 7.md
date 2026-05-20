---
tags: [networking, week-7]
week: 7
---

# Week 7 - Inter-VLAN Routing

## Topics Introduced This Week

- [[Inter-VLAN Routing]] - Sub-interfaces, Router-on-a-Stick (ROAS), Layer 3 switch SVIs

## Key Concepts

**Layer 2 cannot route between VLANs on its own** - a router (or Layer 3 switch) is always required. Without it, VLANs are completely isolated.

**Router-on-a-Stick (ROAS)** uses a single physical router port configured with multiple sub-interfaces, each assigned to a VLAN. The router trunk-connects to the switch and handles inter-VLAN routing over that one link.

**Layer 3 switches** can do this natively using Switch Virtual Interfaces (SVIs), which are more efficient since routing happens in hardware.

## Connections to Other Weeks

- VLAN tagging (802.1q) from [[VLANs]] (Week 3) is required for trunk links used in ROAS
- The routing table and packet traversal concepts come from [[IPv4 Routing]] (Week 6)
- ARP is used within each VLAN - see [[ARP (Address Resolution Protocol)]] (Week 4)
