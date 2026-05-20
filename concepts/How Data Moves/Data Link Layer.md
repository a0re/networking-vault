---
tags: [networking, layer-2, data-link, week-1, week-2]
week: 1
aliases: [Layer 2, Data Link, LLC, MAC sublayer]
---

# Data Link Layer

> **Source:** [[Week 1]], [[Week 2]]

## Purpose

Layer 2 is the **boundary between software and physical hardware**. It:
- Allows upper layers (software) to access the physical medium
- Accepts **packets** from Layer 3 and packages them into **frames** (the Layer 2 PDU)
- Handles **media access control** - coordinating which node transmits when on a shared medium
- Performs **error detection** on received frames

---

## Two Sublayers

The Data Link Layer is divided into two sublayers so that upper-layer protocols are decoupled from the physical medium:

### LLC - Logical Link Control (Upper sublayer, software)
- Interfaces with Layer 3 protocols
- Adds header info to identify the Layer 2 protocol
- Allows **multiple Layer 3 protocols** (IPv4, IPv6) to share the same physical interface simultaneously
- Implemented in software (driver level)

### MAC - Media Access Control (Lower sublayer, hardware)
- Provides **Layer 2 addressing** (MAC addresses)
- Encapsulates packets into frames formatted for the specific medium
- Handles the rules for **accessing the shared medium** (CSMA/CD, CSMA/CA)
- Implemented in hardware (NIC)

> [!note] The LLC/MAC split is why the same IPv4 packet can travel over Ethernet, Wi-Fi, and Bluetooth without any changes to the packet itself - only the frame changes at each hop.

---

## Frame Structure

```
[ Frame Header ] [ Packet / Payload ] [ Frame Trailer ]
```

| Field | Purpose |
|-------|---------|
| **Frame Start** | Indicates start (and end) of frame |
| **Addressing** | Source and destination MAC addresses |
| **Type** | Layer 3 protocol carried (IPv4, IPv6, etc.) |
| **Quality Control** | QoS - priority level (e.g. for voice traffic) |
| **Data** | The packet payload |
| **Error Detection** | FCS / CRC checksum value |

> [!important] Point-to-point links don't need addressing fields (only 2 nodes). Ethernet and shared-media links require MAC addressing.

---

## Media Access Control

When multiple nodes share a medium, they need rules to avoid collisions:

### CSMA/CD - Carrier Sense Multiple Access with Collision Detection
*Used by wired Ethernet (802.3)*

1. Listen to the medium
2. If idle, begin transmitting
3. While transmitting, continue listening for collisions
4. If collision detected → send jamming signal so all nodes detect it → back off for a random period → retry

### CSMA/CA - Carrier Sense Multiple Access with Collision Avoidance
*Used by wireless (802.11)*

1. Listen to the medium
2. If busy, back off and wait
3. If clear, send a notification (request to send)
4. A controller node (access point) grants permission
5. Transmit - collisions are still possible but reduced

### Controlled Access (Legacy)
- Only one node transmits at a time using a **token**
- The token is passed around; a node can only transmit if it holds the token
- No collisions, but **inefficient** - nodes wait for the token even when the medium is free
- Example: FDDI (Fibre Distributed Data Interface)

---

## Error Detection

The Data Link Layer detects (but does not correct) errors:
- A **numerical value (CRC/FCS)** is calculated from the frame data and appended as a trailer
- The receiver performs the same calculation - if values don't match, the frame is **dropped silently**
- Upper layers handle retransmission (the Data Link Layer just discards corrupt frames)

---

## Network Topologies

**Physical topology** - how nodes are physically connected by media.

**Logical topology** - how the Data Link Layer arranges data flow, independent of physical wiring.

| WAN Topology | Description |
|-------------|-------------|
| **Point-to-Point** | Direct link between 2 nodes; no addressing needed; no collisions possible |
| **Hub and Spoke** | Central hub with point-to-point links to each spoke |
| **Full Mesh** | Every node connected to every other node |

---

## Duplex Modes

| Mode | Description |
|------|-------------|
| **Half Duplex** | One direction at a time - send *or* receive, not simultaneously |
| **Full Duplex** | Both directions simultaneously - separate send and receive paths |

> [!warning] Both ends of a link must agree on duplex mode. A mismatch causes collisions and severe performance degradation.

---

## Per-Hop Behaviour

At each router (Layer 3 hop), the frame is **completely stripped and rebuilt**:
- The old Layer 2 header is removed
- A new frame header is created for the outgoing medium
- The payload (packet) is unchanged
- MAC addresses change at every hop; IP addresses do not

---

## Related Notes

- [[Protocol Models (OSI & TCP-IP)]] - Layer 2 in the model
- [[Data Encapsulation]] - how frames wrap packets
- [[Ethernet & MAC Addressing]] - Ethernet's specific implementation of Layer 2
- [[Collision Domains & Switching]] - how switches manage Layer 2 domains
- [[VLANs]] - logical partitioning of Layer 2
- [[Physical Layer]] - Layer 1 below Data Link
