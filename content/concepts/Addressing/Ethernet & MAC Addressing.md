---
tags: [networking, ethernet, mac-address, layer-2, week-2]
week: 2
aliases: [Ethernet, MAC Address, MAC, 802.3, Unicast, Broadcast, Multicast]
---

# Ethernet & MAC Addressing

> **Source:** [[Week 2]]

## History

Ethernet is based on the **ALOHA network** - the first wireless packet radio network - which pioneered shared-media access control. The first Ethernet communication standard was released in 1980. Despite speed increasing from 10 Mb/s to 400 Gb/s, the **frame format has remained consistent** - backward compatibility is preserved.

Modern Ethernet switches no longer strictly *require* media access control (each port is its own collision domain), but the mechanism is still defined for backward compatibility.

---

## Ethernet Standards

Ethernet is a **family of standards**, not a single protocol. Two IEEE families cover it:

| Standard | Layer | Covers |
|----------|-------|--------|
| **802.2** | Data Link (LLC) | Logical link control, software/upper-layer interaction |
| **802.3** | Data Link (MAC) + Physical | Media access control, framing, physical layer specs |

802.3 has multiple sub-specifications for different media (copper, fibre) and speeds.

---

## MAC Address

A **MAC address** is a Layer 2 hardware identifier burned into a NIC by the manufacturer.

| Property | Value |
|----------|-------|
| Length | 48 bits (6 bytes) |
| Format | 12 hexadecimal digits (e.g. `A4-C3-F0-12-AB-FF`) |
| Scope | **Locally significant** - only meaningful within a Layer 2 network segment |

### Structure

```
[ OUI (3 bytes) ][ NIC Identifier (3 bytes) ]
  ^                ^
  Vendor ID        Unique per device, assigned by vendor
  (IEEE-assigned)
```

**OUI** = Organizationally Unique Identifier - the first 3 bytes identify the manufacturer. Regulated by IEEE.

---

## Ethernet Frame Fields

### Ethernet II (used in TCP/IP networks)

| Field | Size | Purpose |
|-------|------|---------|
| **Preamble** | 7 bytes | Synchronisation signal - receiver locks onto transmission rate |
| **SFD** | 1 byte | Start Frame Delimiter - signals frame start |
| **Destination MAC** | 6 bytes | Who receives this frame |
| **Source MAC** | 6 bytes | Who sent this frame |
| **EtherType** | 2 bytes | Layer 3 protocol in the payload (0x0800 = IPv4, 0x86DD = IPv6) |
| **Data** | 46–1500 bytes | The packet payload |
| **FCS** | 4 bytes | Frame Check Sequence - CRC error detection |

> [!note] **Destination MAC is listed first** - the receiver checks it immediately and discards the frame if it's not addressed to them, before reading anything else.

### Data field size limits

- **Maximum 1500 bytes** - prevents one node monopolising the medium
- **Minimum 46 bytes** - required for collision detection. If data < 46 bytes, padding is added.

The minimum exists because CSMA/CD requires the frame to be long enough that both nodes at maximum cable distance can detect each other's transmission and thus detect a collision. These constraints are now legacy but kept for backwards compatibility.

---

## Address Types

### Unicast
- One sender → one specific receiver
- Destination MAC is the unique address of the target NIC
- Used for normal directed communication

### Broadcast
- One sender → **all nodes** on the network
- Destination MAC: `FF-FF-FF-FF-FF-FF` (all bits set to 1)
- Destination IP: `255.255.255.255` (limited) or network broadcast address (directed)
- All nodes receive and process the frame
- Routers do **not** forward broadcasts by default

### Multicast
- One sender → a **selected group** of receivers
- Uses reserved multicast MAC addresses (mapped from multicast IP range `224.x.x.x–239.x.x.x`)

---

## Related Notes

- [[Data Link Layer]] - the Layer 2 context for Ethernet
- [[Collision Domains & Switching]] - how switches handle Ethernet frames
- [[ARP (Address Resolution Protocol)]] - uses Ethernet broadcast to resolve IP → MAC
- [[VLANs]] - 802.1q tagging extends the Ethernet frame with a VLAN ID
