---
tags: [networking, week-8]
week: 8
---

# Week 8 - Layer 2 Redundancy & Spanning Tree Protocol

## Topics Introduced This Week

- [[Layer 2 Redundancy]] - Why redundancy matters, broadcast storms, unicast duplication
- [[Spanning Tree Protocol (STP)]] - BID, root bridge election, root ports, designated ports, port states
- [[STP Advanced (PVST+ & RSTP)]] - Per-VLAN spanning tree, Rapid STP, edge ports, BPDU guard

## Key Concepts

**Without STP, redundant switch paths cause broadcast storms.** Ethernet frames have no TTL, so a broadcast on a looped network circulates forever.

**STP's three-step algorithm**: elect a root bridge (lowest BID/MAC), elect root ports on non-root bridges (cheapest path to root), then designate or block remaining ports. Blocked ports break the loop but the physical path is preserved for failover.

**Rapid PVST+** is the modern preferred standard - it converges much faster than classic 802.1D STP and runs a separate spanning tree instance per VLAN.

## Connections to Other Weeks

- Broadcast storms are caused by the same broadcast forwarding behaviour seen in [[Collision Domains & Switching]] (Week 2)
- PVST+ is per-VLAN, so it ties directly into [[VLANs]] (Week 3)
- Link Aggregation (Week 9) also interacts with STP - see [[Link Aggregation (EtherChannel)]]
