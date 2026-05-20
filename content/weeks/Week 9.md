---
tags: [networking, week-9]
week: 9
---

# Week 9 - Link Aggregation (EtherChannel)

## Topics Introduced This Week

- [[Link Aggregation (EtherChannel)]] - EtherChannel concepts, PAgP vs LACP, configuration guidelines

## Key Concepts

**EtherChannel bundles multiple physical links into one logical link.** STP sees it as a single link, so no ports get blocked for loop prevention - you get both redundancy and bandwidth aggregation.

**LACP (802.3ad)** is the open standard and preferred over Cisco's proprietary PAgP. LACP Active/Passive modes determine who initiates negotiation.

**Important limitation**: a single Ethernet frame always travels on one physical link - you get load balancing across flows, not true bonded throughput for a single stream.

## Connections to Other Weeks

- STP interaction is key - EtherChannel appears as one link to [[Spanning Tree Protocol (STP)]] (Week 8), enabling faster convergence
- Trunk operation across EtherChannel bundles ties into [[VLANs]] (Week 3)
