---
tags: [networking, etherchannel, lacp, pagp, link-aggregation, layer-2, week-9]
week: 9
aliases: [EtherChannel, Link Aggregation, LACP, PAgP, Port Channel, LAG]
---

# Link Aggregation (EtherChannel)

> **Source:** [[Week 9]]

## What Is Link Aggregation?

Link aggregation (EtherChannel on Cisco) bundles **multiple physical links** between two switches into a **single logical link**. The benefits are:

- **Higher effective bandwidth** - multiple 1 Gb/s links appear as one higher-bandwidth link
- **Redundancy** - if one physical link fails, traffic shifts to remaining links automatically
- **STP simplification** - STP sees one logical link, so it doesn't block any ports in the bundle
- **Simpler management** - configure the channel once, not each interface separately

---

## Important Limitations

- A **single Ethernet frame always travels on one physical link** - no frame is split across links
- Full aggregate throughput requires **multiple concurrent flows** spread across links
- **Serialisation delay** is the same as a single link (no speed-of-light improvement)
- All links in a bundle must be the **same interface type** (speed, duplex, media)
- Maximum **16 ports** per EtherChannel bundle (Cisco)
- All ports must have **matching VLAN and trunk configuration**

> [!note] EtherChannel gives you load balancing across flows, not bonded throughput for a single flow. A file transfer on one TCP connection will only ever use one physical link.

---

## Link Aggregation Protocols

### LACP - Link Aggregation Control Protocol (IEEE 802.3ad)

Open standard - works between different vendors.

| Mode | Behaviour |
|------|-----------|
| **Active** | Actively sends LACP negotiation frames - initiates the channel |
| **Passive** | Waits for the other side to initiate |
| **On** | Forces the channel without negotiation (no LACP frames) |

> **Active + Active** or **Active + Passive** → EtherChannel forms  
> **Passive + Passive** → EtherChannel does NOT form (neither initiates)

### PAgP - Port Aggregation Protocol (Cisco proprietary)

Cisco-only - cannot use with non-Cisco equipment. **Prefer LACP.**

| Mode | Behaviour |
|------|-----------|
| **Desirable** | Actively tries to form a channel |
| **Auto** | Passively waits |
| **On** | Forces without negotiation |

---

## STP Interaction

[[Spanning Tree Protocol (STP)]] sees the EtherChannel bundle as a **single logical link**, not multiple links. This means:

- No ports in the bundle are blocked by STP
- STP convergence is simpler and faster
- If a physical link in the bundle fails, STP does **not** need to recalculate - the logical link still exists

This is a major advantage over using raw redundant links, where STP would block all but one.

---

## Configuration Guidelines (Cisco)

Before configuring, ensure both switches have:

- [ ] EtherChannel support
- [ ] **Same speed and duplex** on all member interfaces
- [ ] **Same VLAN** membership on all interfaces
- [ ] **Same trunk configuration** (native VLAN, allowed VLANs) if trunking
- [ ] Same port type (all copper, all fibre)

### Configuration Steps

```
! Shutdown one side first to avoid loops during config
interface range GigabitEthernet0/1 - 2
  shutdown

! Assign to a port channel
channel-group 1 mode active    (LACP)
! or
channel-group 1 mode desirable (PAgP)

! Bring up interfaces
no shutdown

! Configure the logical port-channel interface
interface port-channel 1
  switchport mode trunk
  switchport trunk allowed vlan 10,20,30
```

---

## Load Balancing

Traffic is distributed across physical links using a **hashing algorithm** based on configurable criteria:
- Source/Destination MAC
- Source/Destination IP
- Source/Destination Port

The hash determines which physical link each flow uses. The same flow always uses the same link - ensuring frame ordering is preserved within a flow.

---

## Related Notes

- [[Spanning Tree Protocol (STP)]] - how STP interacts with EtherChannel
- [[STP Advanced (PVST+ & RSTP)]] - PVST+ per-VLAN consideration for EtherChannel
- [[VLANs]] - VLAN and trunk config must match across the bundle
- [[Layer 2 Redundancy]] - the redundancy problem EtherChannel addresses
