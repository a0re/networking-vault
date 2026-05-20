---
tags: [networking, layer-3, ip, network-layer, week-4]
week: 4
aliases: [Layer 3, Network Layer, IP, Internet Protocol]
---

# Network Layer (IP)

> **Source:** [[Week 4]]

## Purpose

The Network Layer (Layer 3) is responsible for **moving packets from source host to destination host** across multiple networks. It:

- Provides **end-to-end connectivity** across interconnected Layer 2 networks
- Assigns and uses **logical (IP) addresses** to identify hosts globally
- **Routes packets** hop-by-hop through the network
- Handles **encapsulation** of transport PDUs into packets
- Is **link-layer agnostic** - works over any Layer 2 technology

---

## Key Properties of IP

### Connectionless

IP does **not** establish a connection before sending data:
- No session setup, acknowledgement, or teardown
- The receiver doesn't know packets are coming
- The sender doesn't know if packets arrived
- Analogy: mailing a letter - you just send it

### Best-Effort Delivery

IP makes **no guarantees**:
- Packets may be dropped, duplicated, or arrive out of order
- No mechanism to detect or recover from packet loss at this layer
- The **Transport Layer** (TCP) handles reliability if needed

> [!note] This design is intentional - simplicity makes IP fast, scalable, and universally applicable. Reliability is added only where needed by upper layers.

### Packet-Based

Each packet is forwarded **independently** through the network:
- Routers make independent forwarding decisions at each hop based on the routing table
- Different packets in the same session may take different paths

---

## Network Layer Responsibilities

1. **Addressing** - assign unique IP addresses to identify every host
2. **Encapsulation** - wrap transport PDUs in a Layer 3 header to form packets
3. **Routing** - determine the best path to the destination at each hop
4. **Decapsulation** - strip the Layer 3 header and deliver the PDU to the correct upper layer

---

## Packet vs Connection-Based Layer 3

| Type | Behaviour |
|------|-----------|
| **Packet-based** (IP) | Each packet routed independently; path may vary; simpler |
| **Connection-based** (e.g. MPLS) | Path negotiated upfront; all packets follow the same route; more overhead but deterministic |

---

## Handling Network Problems

| Problem | IP Behaviour | Who Handles It |
|---------|-------------|----------------|
| Packet loss | Drops silently | Transport Layer (TCP) |
| Duplication | Forwards duplicates | Transport Layer |
| Routing failure | Routing protocols recalculate | Routing protocols (OSPF, EIGRP) |
| Congestion/performance | No QoS guarantee | QoS techniques, upper layers |

---

## Network Layer Protocols

| Protocol | Status |
|----------|--------|
| IPv4 | Current (still dominant) |
| IPv6 | Current (growing) |
| IPX | Legacy |
| AppleTalk | Legacy |
| CLNS/DECnet | Legacy |

---

## Related Notes

- [[IPv4 Addressing]] - IP address structure, classes, subnetting, header fields
- [[ARP (Address Resolution Protocol)]] - resolves IP to MAC at Layer 2
- [[IPv4 Routing]] - how routers forward packets using routing tables
- [[Data Link Layer]] - the Layer 2 that carries Layer 3 packets
- [[Data Encapsulation]] - how packets fit into the PDU hierarchy
