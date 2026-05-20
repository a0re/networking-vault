---
tags: [networking, stp, spanning-tree, layer-2, week-8]
week: 8
aliases: [STP, Spanning Tree, BID, Bridge ID, Root Bridge, Root Port, Designated Port, BPDU]
---

# Spanning Tree Protocol (STP)

> **Source:** [[Week 8]]

## Purpose

STP prevents [[Layer 2 Redundancy#The Problem with Layer 2 Redundancy|Layer 2 broadcast storms and loops]] by ensuring there is **only one active logical path between any two devices** in a switched network - while keeping redundant physical paths available for failover.

**How it works**: STP identifies and **blocks** ports on redundant paths. If the active path fails, the blocked port transitions to forwarding, restoring connectivity.

---

## Bridge ID (BID)

Every switch has a **Bridge ID** used in STP elections:

```
[ Bridge Priority (2 bytes) ][ MAC Address (6 bytes) ]
       Total: 8 bytes
```

| Field | Default | Notes |
|-------|---------|-------|
| **Bridge Priority** | 32,768 | Configurable in multiples of 4096 |
| **MAC Address** | NIC MAC | Used as tiebreaker |

The BID determines which switch becomes the **Root Bridge**.

---

## Spanning Tree Algorithm (STA) - Three Steps

### Step 1: Elect a Root Bridge

- All switches start by advertising their BID via **BPDU** (Bridge Protocol Data Unit) frames
- The switch with the **lowest Bridge Priority** wins
- If priorities are equal, the switch with the **lowest MAC address** wins
- The root bridge is the reference point for all STP calculations

> [!tip] To control which switch becomes root, manually lower its bridge priority with `spanning-tree vlan 1 root primary`.

### Step 2: Elect Root Ports (one per non-root switch)

Every non-root switch must elect one **Root Port** - the port with the cheapest path back to the root bridge.

**Path cost** is based on link speed:

| Link Speed | STP Cost |
|------------|----------|
| 10 Gb/s | 2 |
| 1 Gb/s | 4 |
| 100 Mb/s | **19** (most common default) |
| 10 Mb/s | 100 |

Tiebreakers (in order):
1. Lowest path cost to root
2. Lowest sender BID
3. Lowest port ID

Each non-root switch has **exactly one** root port. The root port can send and receive traffic.

### Step 3: Elect Designated & Non-Designated Ports

For every **link segment** in the network:
- One end must be a **Designated Port** (active, forwards traffic)
- The other end may become a **Non-Designated (Blocked) Port**

**Rules:**
- All ports on the **root bridge** are designated ports
- The port at the other end of a root port's link is a designated port
- For remaining links: the switch with the **lower path cost to root** gets the designated port. If tied, lower BID wins.
- Ports that lose the designated port election become **blocked**

---

## STP Port States

| State | Can Forward Data? | Can Learn MACs? | Notes |
|-------|-------------------|-----------------|-------|
| **Disabled** | No | No | Administratively shut down |
| **Blocking** | No | No | Receives BPDUs only |
| **Listening** | No | No | Participating in STP election |
| **Learning** | No | **Yes** | Building MAC table before forwarding |
| **Forwarding** | **Yes** | Yes | Normal operation |

> [!note] BPDUs are **always** processed, even on blocked ports. This allows STP to detect topology changes.

---

## STP Convergence

**Convergence** = the time it takes for all switches to agree on the topology and settle into stable port states.

Classic 802.1D STP convergence is **slow** (~30–50 seconds) because ports must pass through Listening (15s) and Learning (15s) states before forwarding.

This is why rapid variants were developed - see [[STP Advanced (PVST+ & RSTP)]].

---

## Verification (Cisco CLI)

```
show spanning-tree
```

Shows:
- Root ID (root bridge's BID)
- Bridge ID (this switch's BID)
- Whether this switch is the root
- Port roles: `Root`, `Desg` (designated), `Altn` (alternate/blocked)
- Port states: `FWD` (forwarding), `BLK` (blocking)

---

## Summary

```
Step 1: Elect Root Bridge    → Lowest BID (priority, then MAC)
Step 2: Elect Root Ports     → Cheapest path to root (cost, BID, port ID)
Step 3: Elect Designated Ports → Active port on each segment
        Remaining ports      → Blocked (non-designated)
```

---

## Related Notes

- [[Layer 2 Redundancy]] - the problem STP solves
- [[STP Advanced (PVST+ & RSTP)]] - PVST+, RSTP, and modern improvements
- [[VLANs]] - PVST+ runs separate STP instances per VLAN
- [[Link Aggregation (EtherChannel)]] - presents as a single link to STP
- [[Collision Domains & Switching]] - how switches and broadcast domains work
