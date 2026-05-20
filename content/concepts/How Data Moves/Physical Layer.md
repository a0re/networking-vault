---
tags: [networking, layer-1, physical, week-1]
week: 1
aliases: [Layer 1, Physical]
---

# Physical Layer

> **Source:** [[Week 1]]

## Purpose

The Physical Layer (Layer 1) has one job: **transmit raw bits** over a physical medium. It accepts frames from the [[Data Link Layer]] above, encodes them into signals, and places them onto the medium.

---

## Two Operating Modes

| Mode | Direction | Action |
|------|-----------|--------|
| **Sending** | Down the stack | Accept frame from Data Link → encode to bits → transmit on medium |
| **Receiving** | Up the stack | Receive signal from medium → decode to bit stream → pass frame to Data Link |

---

## Physical Media Types

- Copper (electrical signal)
- Fibre optic (light pulses)
- Wireless / radio waves

Signals are placed **one at a time** onto the medium. The receiver decodes the signal back into a bit stream.

---

## Bandwidth, Throughput & Goodput

These three terms are often confused:

| Term | Definition |
|------|-----------|
| **Bandwidth** | Maximum capacity of the link (bits per second) - the theoretical ceiling |
| **Throughput** | Actual transmission rate over a period of time - always ≤ bandwidth |
| **Goodput** | Rate of *useful* data transmitted - excludes overhead (headers, acknowledgements, retransmissions) |

> [!tip] Goodput is what the application actually gets. It's always less than throughput, which is always less than bandwidth.

---

## Encoding

**Encoding** converts a stream of bits into a predefined code for transmission.
**Signalling** physically represents that bit stream on the medium (voltage levels, light pulses, radio amplitude/frequency).

### Manchester Encoding *(optional/legacy)*

A method of encoding digital data where each bit is represented by a signal **transition**:

| Bit | Signal Transition |
|-----|-------------------|
| 0 | High → Low |
| 1 | Low → High |

The transition itself carries the data, making it self-clocking - the receiver can recover timing from the signal alone.

---

## Related Notes

- [[Protocol Models (OSI & TCP-IP)]] - where Layer 1 fits
- [[Data Encapsulation]] - bits are the final PDU after full encapsulation
- [[Data Link Layer]] - the layer that hands frames down to Physical
