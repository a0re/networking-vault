---
tags: [networking, routing, ipv4, layer-3, week-6]
week: 6
aliases: [Routing, Default Gateway, Routing Table, Packet Traversal, Hop]
---

# IPv4 Routing

> **Source:** [[Week 6]]

## Overview

Routing is how packets move **between different networks**. Routers exchange routing information with each other to build tables of known destinations and decide the best path for each packet.

End devices don't need to know the full topology - they just need to know their **default gateway**.

---

## The Default Gateway

Every host has a local routing table with three types of entries:

| Entry Type | What It Covers |
|------------|---------------|
| **Direct connection (L)** | The host's own IP address on that interface |
| **Local network route (C)** | The host's directly connected subnet |
| **Default route** | Everything else → send to the gateway |

The default gateway is the router's IP address on the local subnet. Any traffic destined for an IP **outside the local subnet** is forwarded to the gateway.

> [!important] To reach the gateway, the host must know the **gateway's MAC address** - resolved via [[ARP (Address Resolution Protocol)]].

---

## When Is a Gateway Required?

| Condition | Gateway Needed? |
|-----------|----------------|
| Same switch, same subnet | No - communicate directly via Layer 2 |
| Same broadcast domain / network portion | No - ARP resolves the MAC directly |
| Different subnet / broadcast domain | **Yes** - must route through the gateway |

---

## Packet Traversal: What Changes at Each Hop

This is a crucial concept - understand what stays the same and what changes as a packet crosses routers:

| Field | Behaviour |
|-------|-----------|
| **Source IP** | Stays the same end-to-end |
| **Destination IP** | Stays the same end-to-end |
| **Source MAC** | Changes at each hop (becomes the router's outgoing interface MAC) |
| **Destination MAC** | Changes at each hop (becomes the next-hop device's MAC) |

```
Host A → Router 1 → Router 2 → Host B

Hop 1 (Host A → Router 1):
  Src IP: A    Dst IP: B
  Src MAC: A   Dst MAC: Router1-interface

Hop 2 (Router 1 → Router 2):
  Src IP: A    Dst IP: B
  Src MAC: Router1-out   Dst MAC: Router2-interface

Hop 3 (Router 2 → Host B):
  Src IP: A    Dst IP: B
  Src MAC: Router2-out   Dst MAC: B
```

> [!tip] Switches in the path don't change MACs - they just forward based on the existing destination MAC.

---

## Routing Table

Routers build local routing tables from:
- **Directly connected** networks (C routes) - auto-populated when interfaces are configured
- **Local interface IPs** (L routes) - the router's own interface addresses
- **Routing protocols** (e.g. OSPF, EIGRP) - learned from neighbouring routers

Each routing table entry specifies:
- The **destination network**
- The **exit interface** and/or **next-hop IP**
- A **metric** (cost) used to prefer one path over another

Routing tables don't store a complete unique path - they store the **best next hop** and let each router make its own independent forwarding decision.

---

## Pathing Decision (Host Perspective)

When a host sends a packet:

1. Compare destination IP against own subnet mask
2. If **same subnet** → ARP for destination MAC directly → send frame
3. If **different subnet** → ARP for default gateway MAC → send frame to gateway
4. Gateway receives the frame, strips L2 header, looks at destination IP
5. Looks up destination in routing table → determines next hop
6. ARPs for next hop MAC → re-encapsulates and forwards

---

## Routing Protocols

Routers use routing protocols to share network topology information:
- If a path goes down, the routing protocol recalculates and updates the routing table
- Examples: OSPF, EIGRP, BGP (mentioned in notes - EIGRP noted as unfamiliar, worth revisiting)

---

## Related Notes

- [[Network Layer (IP)]] - Layer 3 fundamentals
- [[ARP (Address Resolution Protocol)]] - resolves next-hop MAC at each router
- [[IPv4 Addressing]] - subnet masks that determine same/different network
- [[Inter-VLAN Routing]] - routing between VLANs on the same physical switch
- [[Data Encapsulation]] - the per-hop decapsulation/re-encapsulation process
