---
tags:
  - networking
  - vlan
  - layer-2
  - week-3
week: 3
aliases:
  - VLAN
  - 802.1q
  - Trunk
  - Access Port
  - Native VLAN
  - DTP
---

# VLANs

> **Source:** [[Week 3]]

## What Is a VLAN?

A **VLAN (Virtual LAN)** is a logical partition of a Layer 2 network. It creates **separate broadcast domains** on the same physical switch infrastructure without requiring separate physical hardware.

- Devices in different VLANs **cannot communicate at Layer 2** - a router is required
- End devices (PCs) are **unaware** they are in a VLAN - they think they're on a regular switch
- VLANs can span multiple physical switches via **trunk links**

> [!important] Traffic can only cross VLAN boundaries via a Layer 3 device (router or Layer 3 switch). See [[Inter-VLAN Routing]].

---

## Why VLANs? - Problems They Solve

**Before VLANs**, to isolate traffic you needed separate physical switches and routers per group - expensive, doesn't scale, and caused inefficient traffic flows.

**VLANs allow**:
- **Security** - isolate departments; a Layer 2 attack in one VLAN doesn't affect others
- **Performance** - smaller broadcast domains mean less unnecessary broadcast traffic
- **Cost reduction** - share physical infrastructure across logical groups
- **Easier administration** - move users between groups by changing port config, not physically moving cables

---

## VLAN ID Ranges

VLANs are identified by a **12-bit field** in the 802.1q header, giving 4096 possible IDs (0–4095):

| Range | Category | Storage |
|-------|----------|---------|
| 0 | Reserved | - |
| **1–1005** | Normal range | Flash memory |
| 1006–4094 | Extended range | Running config |
| 4095 | Reserved | - |

VLAN 1 is the **default VLAN** on Cisco switches - it cannot be deleted or renamed, and carries all Layer 2 control traffic by default.

---

## Port Types

### Access Ports
- Carry traffic for **one VLAN only**
- Connected to end devices (PCs, printers)
- Frames enter the switch **untagged** (the device doesn't know about VLANs)
- The switch **inserts** the VLAN tag internally for processing
- The switch **removes** the tag before sending out to the end device

```
PC → [untagged] → Switch port → [tagged internally] → switch processing → [untagged] → PC
```

### Trunk Ports
- Carry traffic for **multiple VLANs simultaneously**
- Connected between switches, or between a switch and a router
- Frames are **tagged** with their VLAN ID using 802.1q
- Used for inter-switch links and Router-on-a-Stick setups

---

## 802.1q Tagging

The IEEE standard for VLAN tagging. A **4-byte tag** is inserted into the Ethernet frame between the Source MAC and the EtherType fields:

```
[ Dest MAC ][ Src MAC ][ 802.1q Tag (4 bytes) ][ EtherType ][ Data ][ FCS ]
                         ↑
                         TPID (0x8100) + Priority + DEI + VLAN ID (12 bits)
```

The last **12 bits** of the tag are the VLAN ID.

### Native VLAN
- Frames on the **native VLAN are not tagged** on trunk links
- Untagged frames received on a trunk are assigned to the native VLAN
- Default native VLAN is VLAN 1 on Cisco
- If a frame arrives on a trunk without a tag and no native VLAN exists for that port, it is **dropped**

> [!warning] Both ends of a trunk link must agree on the native VLAN, or traffic will be misassigned.

---

## VLAN Membership

| Type | How It Works |
|------|-------------|
| **Static** | Administrator manually assigns ports to VLANs |
| **Dynamic** | VMPS (VLAN Membership Policy Server) assigns VLANs based on source MAC address |

---

## Dynamic Trunking Protocol (DTP)

DTP allows switches to **negotiate trunk links automatically**. Cisco proprietary.

| Mode | Behaviour |
|------|-----------|
| **Dynamic Auto** | Listens for DTP; will become a trunk if the other end initiates |
| **Dynamic Desirable** | Actively tries to negotiate a trunk |
| **Trunk** | Forcibly set as trunk - doesn't negotiate |
| **Access** | Forcibly set as access - doesn't negotiate |
| **Nonegotiate** | Disables DTP completely; no negotiation frames sent or received |

> [!warning] If both ends are set to **Dynamic Auto**, neither initiates - the link becomes an access link. Two `Dynamic Auto` ports will never form a trunk.

---

## VLAN Types

| Type | Purpose | Default (Cisco) |
|------|---------|----------------|
| **Data VLAN** | Carries user/application traffic | Any created VLAN |
| **Default VLAN** | Unconfigured switch default | VLAN 1 |
| **Native VLAN** | Carries untagged trunk traffic | VLAN 1 |
| **Management VLAN** | Administrative access (SSH, Telnet) | VLAN 1 |
| **Voice VLAN** | Dedicated for VoIP traffic with QoS | Configured separately |

---

## Intra-VLAN Communication

When two hosts in the **same VLAN** communicate:
- This is a Layer 2 operation - the switch forwards based on MAC address
- An ARP broadcast is sent within the VLAN broadcast domain
- The router also receives the ARP (it's on the same broadcast domain) but only replies if it's the target

---

## Related Notes

- [[Data Link Layer]] - Layer 2 foundations
- [[Ethernet & MAC Addressing]] - the frame format VLANs extend
- [[Collision Domains & Switching]] - broadcast domains that VLANs partition
- [[VLAN Security]] - attacks and hardening
- [[Inter-VLAN Routing]] - how traffic crosses VLAN boundaries
- [[STP Advanced (PVST+ & RSTP)]] - per-VLAN spanning tree
