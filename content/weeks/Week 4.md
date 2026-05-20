---
tags: [networking, week-4]
week: 4
---

# Week 4 - Network Layer, IPv4 & ARP

## Topics Introduced This Week

- [[Network Layer (IP)]] - Layer 3 responsibilities, best-effort delivery, packet vs connection based
- [[IPv4 Addressing]] - Classful addressing, subnet masks, public/private ranges, special addresses, IPv4 header fields, fragmentation, unicast/broadcast/multicast
- [[ARP (Address Resolution Protocol)]] - Resolving IP to MAC, ARP cache, internal vs external network ARP behaviour

## Key Concepts

**IP is connectionless and best-effort** - it makes no guarantees. Reliability is left to upper layers (TCP). This simplicity is what makes IP fast and universally usable.

The **IPv4 header** carries critical fields including TTL (prevents infinite loops), Protocol (tells receiver what's inside), and fragmentation fields (ID, Flags, Offset) for splitting packets across links with small MTUs.

**ARP** is the glue between Layer 2 and Layer 3. Even though a packet has an IP destination, it can only travel one hop at a time using MAC addresses. ARP resolves the MAC for the next hop.

## Connections to Later Weeks

- ARP behaviour across routed networks is central to [[IPv4 Routing]] (Week 6)
- The concept of the default gateway introduced here is expanded in [[IPv4 Routing]]
- Fragmentation connects to MTU, which relates to [[Ethernet & MAC Addressing]] frame size limits (Week 2)
