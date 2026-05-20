---
tags: [networking, stp, pvst, rstp, layer-2, week-8]
week: 8
aliases: [PVST+, RSTP, Rapid STP, Rapid PVST+, RPVST+, 802.1w, 802.1D, BPDU Guard, PortFast]
---

# STP Advanced (PVST+ & RSTP)

> **Source:** [[Week 8]]

## STP Variants Comparison

| Protocol | Standard | Spanning Trees | Convergence |
|----------|----------|----------------|-------------|
| **STP** | IEEE 802.1D | One for entire network | Slow (~30–50s) |
| **PVST+** | Cisco proprietary | One per VLAN | Slow |
| **RSTP** | IEEE 802.1w | One for entire network | **Fast** |
| **Rapid PVST+** | Cisco proprietary | One per VLAN | **Fast** |

> [!tip] **Rapid PVST+** is the preferred protocol in modern Cisco networks.

---

## PVST+ (Per-VLAN Spanning Tree Plus)

PVST+ runs an **independent 802.1D STP instance for each VLAN**. This enables:

- **Load balancing** - different VLANs can have different root bridges and thus use different paths
- **Utilising otherwise blocked links** - a port blocked in VLAN 10's spanning tree can be forwarding in VLAN 20's

**Cost:**
- Higher CPU usage (one STP process per VLAN)
- More bandwidth consumed (one set of BPDUs per VLAN on trunk links)

### Extended System ID

To make BIDs unique per VLAN, PVST+ extends the Bridge Priority field:

```
[ STP Priority (4 bits) ][ Extended System ID = VLAN ID (12 bits) ][ MAC (6 bytes) ]
```

This means fewer priority values are available (multiples of **4096** instead of 1).

**Default Bridge Priority**: VLAN 1 = **32768** (STP) or **32769** (PVST+, which adds the VLAN ID of 1)

### Configuring Root Bridge Priority (Cisco)

```
spanning-tree vlan 1 root primary    → sets priority to 24576
spanning-tree vlan 1 root secondary  → sets priority to 28672
```

Verify with `show spanning-tree` - look for "This bridge is the root."

---

## RSTP (Rapid Spanning Tree Protocol) - 802.1w

RSTP supersedes classic 802.1D and is **backward compatible** with it.

Key improvements:
- **Fast convergence** - seconds instead of 30–50 seconds
- **No blocking state** - replaced by:

| RSTP Port State | Description |
|----------------|-------------|
| **Discarding** | Not forwarding data (replaces Blocking + Listening) |
| **Learning** | Building MAC table, not yet forwarding |
| **Forwarding** | Normal operation |

### New Port Role: Alternate Port

The **Alternate Port** (in Discarding state) is a designated backup to the root port. If the root port fails, the alternate port immediately transitions to forwarding - no waiting through Listening/Learning.

---

## Rapid PVST+

Combines RSTP's fast convergence with PVST+'s per-VLAN instances:
- One fast-converging spanning tree per VLAN
- Best of both worlds
- **This is the current Cisco default and recommended standard**

---

## Special Port Features

### PortFast

Configured on **access ports** connected to end devices (PCs, servers - never other switches):
- Port immediately transitions from discarding/blocking → forwarding
- Skips the Listening and Learning states
- Saves the ~30 second delay when a PC first connects

> [!warning] Never enable PortFast on ports connected to other switches - if a loop forms, STP won't catch it in time.

### Edge Ports (RSTP)

The RSTP equivalent of PortFast. Ports connected to end devices that will **never connect to another switch**. Immediately transitions to forwarding.

### BPDU Guard

When enabled on a PortFast/Edge port, if a **BPDU is received** (meaning another switch was connected), the port is immediately put into **error-disabled state**.

- Protects against accidental or malicious switch connections on user ports
- Port must be manually re-enabled after investigation: `shutdown` then `no shutdown`

```
spanning-tree portfast
spanning-tree bpduguard enable
```

---

## Load Balancing with PVST+

By configuring different root bridges per VLAN, you can distribute traffic across multiple uplinks:

```
VLAN 10 → Root on Switch A → traffic flows via Link 1
VLAN 20 → Root on Switch B → traffic flows via Link 2
```

Without this, all VLANs would share one root and potentially leave redundant links unused.

---

## Related Notes

- [[Spanning Tree Protocol (STP)]] - classic STP fundamentals (start here)
- [[VLANs]] - the per-VLAN context for PVST+
- [[Layer 2 Redundancy]] - why STP exists
- [[Link Aggregation (EtherChannel)]] - an alternative that avoids the STP complexity entirely
