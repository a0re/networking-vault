---
tags: [networking, week-1]
week: 1
---

# Week 1 - Protocol Models, Data Link & Physical Layer

## Topics Introduced This Week

- [[Protocol Models (OSI & TCP-IP)]] - TCP/IP layers, reference models, layered benefits
- [[Data Encapsulation]] - PDUs: Data → Segment → Packet → Frame → Bits
- [[Data Link Layer]] - Layer 2 overview, LLC & MAC sublayers, frame format
- [[Physical Layer]] - Encoding, signalling, bandwidth vs throughput vs goodput

## Key Concepts

The core idea introduced this week is **layered architecture** - each layer has a specific job and communicates only with the layer above and below it. This is why changing a physical medium (copper → fibre) doesn't require rewriting application software.

The **Data Link Layer** is the boundary between software and hardware. It accepts packets from Layer 3 and packages them into **frames** suited to the physical medium. It has two sublayers:
- **LLC** (software) - interfaces with upper layers
- **MAC** (hardware) - handles addressing and framing for the specific medium

## Connections to Later Weeks

- The frame format introduced here is extended by [[Ethernet & MAC Addressing]] (Week 2)
- MAC addressing becomes critical in [[VLANs]] (Week 3) and [[ARP (Address Resolution Protocol)]] (Week 4)
- The concept of decapsulation at each hop underpins [[IPv4 Routing]] (Week 6)
