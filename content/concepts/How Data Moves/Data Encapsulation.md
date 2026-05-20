---
tags: [networking, encapsulation, pdu, week-1]
week: 1
aliases: [PDU, Protocol Data Unit, Encapsulation]
---

# Data Encapsulation

> **Source:** [[Week 1]]

## What Is Encapsulation?

Each layer of the protocol stack **wraps the data from the layer above** with its own header (and sometimes trailer) before passing it down. This is called encapsulation. The reverse - stripping headers on the receiving end - is **decapsulation**.

---

## Protocol Data Units (PDUs)

Each layer has its own name for the unit of data it works with:

| PDU | Layer | Structure |
|-----|-------|-----------|
| **Data** | Application / Presentation / Session | Raw application data |
| **Segment** | Transport | `[Transport Header][Data]` |
| **Packet** | Network | `[Network Header][Transport Header][Data]` |
| **Frame** | Data Link | `[Frame Header][Network Header][Transport Header][Data][Frame Trailer]` |
| **Bits** | Physical | Raw binary signal on the wire |

> [!note] Headers are prepended (placed before) the data so the receiver can identify addressing and routing information before processing the payload.

---

## Encapsulation Flow (Sending)

```
Application Data
  → Transport adds header → Segment
  → Network adds header → Packet
  → Data Link adds header + trailer → Frame
  → Physical encodes to bits → transmitted on media
```

## Decapsulation Flow (Receiving)

```
Bits received from media
  → Physical decodes → Frame
  → Data Link strips frame header/trailer → Packet
  → Network strips network header → Segment
  → Transport strips transport header → Data
  → Application receives data
```

---

## Why Encapsulation Matters

At every **hop** (router), the Layer 2 frame is **stripped and rebuilt** for the next link - the MAC addresses change. But the Layer 3 packet (IP addresses) passes through unchanged. This is what allows a packet to traverse different media types (Ethernet → Fibre → Wireless) without the application knowing.

---

## Related Notes

- [[Protocol Models (OSI & TCP-IP)]] - the layers where each PDU lives
- [[Data Link Layer]] - frame structure in detail
- [[Physical Layer]] - how bits are transmitted
- [[IPv4 Routing]] - where per-hop decapsulation/re-encapsulation happens in practice
