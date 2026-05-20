---
tags: [networking, switching, collision-domain, mac-table, week-2]
week: 2
aliases: [Switching, MAC Table, CAM Table, Collision Domain, Bridge, Frame Forwarding]
---

# Collision Domains & Switching

> **Source:** [[Week 2]]

## Collision Domains

A **collision domain** is any portion of a network where two devices transmitting simultaneously will cause a collision. On a shared medium (like a hub), all connected devices share one collision domain.

The number of devices in a collision domain matters: **more devices = more collisions = worse performance**.

---

## Bridges (Historical Context)

A **bridge** connects two Ethernet segments and learns MAC addresses by observing traffic. It:
- Creates a **micro-segment** between two computers with full-duplex operation
- Eliminates collisions between the two segments
- Learns which MACs are on which side and filters traffic accordingly

Switches are essentially multi-port bridges - the same concept, scaled up.

---

## How a Switch Works

Each **switch port** is its own isolated collision domain. With only one device per port in full-duplex mode, collisions are impossible.

### MAC Address Table (CAM Table)

The switch builds its MAC address table dynamically by **observing source MAC addresses** on incoming frames:

1. Frame arrives on Port 3 with Source MAC `AA:BB:CC:DD:EE:FF`
2. Switch creates/updates entry: `AA:BB:CC:DD:EE:FF → Port 3`
3. Future frames destined for `AA:BB:CC:DD:EE:FF` are forwarded only to Port 3

### Frame Forwarding Decision

| Situation | Switch Action |
|-----------|--------------|
| Destination MAC is in the table | Forward out the specific port only |
| Destination MAC is NOT in the table | **Flood** - forward out all ports except the incoming port |
| Destination MAC is `FF:FF:FF:FF:FF:FF` (broadcast) | Forward out all ports |

> [!important] All switch ports belong to the same **broadcast domain** by default - broadcasts go everywhere. VLANs are used to subdivide broadcast domains. See [[VLANs]].

### ARP + Switch Learning Scenario

1. PC1 wants to reach PC3 but doesn't know PC3's MAC
2. PC1 sends an **ARP broadcast** (`FF:FF:FF:FF:FF:FF`)
3. Switch receives it, records PC1's MAC → Port 1 in the table, floods to all ports
4. PC3 receives the ARP, replies with a **unicast** frame back to PC1
5. Switch receives PC3's reply, records PC3's MAC → Port 3
6. Switch forwards the reply to Port 1 (PC1)
7. Future PC1 ↔ PC3 frames are forwarded directly - no flooding needed

---

## Frame Forwarding Methods

Switches use different strategies with different speed/reliability tradeoffs:

| Method | Behaviour | Latency | Risk |
|--------|-----------|---------|------|
| **Store & Forward** | Receives entire frame, verifies CRC, then forwards | Highest | Never forwards corrupt frames |
| **Cut-Through (Fast-Forward)** | Reads destination MAC only, forwards immediately | Lowest | Can forward corrupt frames |
| **Fragment-Free** | Waits for first 64 bytes (collision window), then forwards | Middle | Catches most collision errors |

> [!tip] **Store & Forward** is safest. **Cut-Through** is fastest. **Fragment-Free** is a practical middle ground - most collisions corrupt the first 64 bytes.

---

## Broadcast Domains

All ports on a switch share one broadcast domain - a broadcast reaches every device. This becomes a problem at scale (too much broadcast traffic).

[[VLANs]] solve this by creating **multiple logical broadcast domains** on one physical switch.

---

## Related Notes

- [[Data Link Layer]] - the Layer 2 context
- [[Ethernet & MAC Addressing]] - frame format and MAC addresses
- [[VLANs]] - how broadcast domains are subdivided
- [[ARP (Address Resolution Protocol)]] - the most common reason for broadcast flooding
- [[Layer 2 Redundancy]] - what happens when you add redundant switch paths
- [[Spanning Tree Protocol (STP)]] - prevents broadcast storms in redundant topologies
