---
tags: [networking, week-6]
week: 6
---

# Week 6 - IPv4 Routing & Default Gateway

## Topics Introduced This Week

- [[IPv4 Routing]] - Default gateway, routing tables, packet traversal across hops, MAC changes at each hop

## Key Concepts

**The default gateway** is the Layer 3 boundary for a host. Any traffic destined for an IP outside the local subnet is sent to the gateway's MAC address (resolved via ARP).

**MAC addresses change at every hop; IP addresses stay the same end-to-end.** This is one of the most important things to understand about how routing works. The router strips the L2 header and re-encapsulates with new source/destination MACs for the next link.

## Connections to Other Weeks

- ARP is used at every hop to resolve the next MAC - see [[ARP (Address Resolution Protocol)]] (Week 4)
- VLAN-separated networks need routing to communicate - see [[Inter-VLAN Routing]] (Week 7)
