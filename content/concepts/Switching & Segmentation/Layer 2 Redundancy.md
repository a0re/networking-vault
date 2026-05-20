---
tags: [networking, redundancy, layer-2, broadcast-storm, week-8]
week: 8
aliases: [Redundancy, Broadcast Storm, Layer 2 Loops, Fault Tolerance]
---

# Layer 2 Redundancy

> **Source:** [[Week 8]]

## Why Redundancy?

Production networks aim for **"five nines" availability** - 99.999% uptime, which allows only ~5.25 minutes of downtime per year. To achieve this, networks cannot rely on a single switch, interface, or cable.

**Redundancy goal**: if any single component fails, traffic continues via an alternate path.

**Design principles:**
- Minimise downtime
- Minimise single points of failure
- Build fault tolerance

---

## The Problem with Layer 2 Redundancy

Unlike Layer 3 (where TTL prevents infinite loops), **Ethernet frames have no Time-To-Live**. If redundant paths exist in a Layer 2 network without a loop-prevention mechanism, traffic circulates forever.

### Broadcast Storms

When a broadcast frame enters a loop topology:

1. Switch A receives the broadcast, floods it to all ports including the redundant link
2. Switch B receives the broadcast, floods it out all ports including back toward Switch A
3. Switch A receives it again, floods again → **infinite loop**

Consequences:
- **Bandwidth saturation** - broadcast traffic consumes all available bandwidth
- **CPU exhaustion** - all devices must process every broadcast
- **MAC table instability** - the same MAC appears on multiple ports simultaneously, causing constant table updates with incorrect entries
- **Network unusable** within seconds

### Unicast Frame Duplication

In a loop topology, unicast frames can also be duplicated:
- The destination device receives multiple copies of the same frame
- Upper-layer protocols (like TCP) are not designed for this and respond by slowing down or resetting connections

---

## The Solution

[[Spanning Tree Protocol (STP)]] was designed specifically to solve Layer 2 loops while preserving the physical redundancy for failover.

STP **logically disables** redundant paths while keeping the physical connections intact - if the active path fails, STP re-enables the blocked path.

---

## Related Notes

- [[Collision Domains & Switching]] - broadcast forwarding behaviour that causes the storm
- [[Spanning Tree Protocol (STP)]] - the solution
- [[STP Advanced (PVST+ & RSTP)]] - modern improvements to STP
- [[Link Aggregation (EtherChannel)]] - an alternative redundancy approach that also avoids loops
