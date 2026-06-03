---
tags:
  - networking
  - udp
  - transport-layer
  - week-11
week: 11
aliases:
  - Week 11b
  - UDP Protocol
  - User Datagram Protocol
---

# Week 11b — UDP: User Datagram Protocol

> **Source:** Lecture 11b — TNE10006/TNE60006 Networks and Switching

## Overview

UDP is a **connectionless, best-effort** transport layer protocol. It provides minimal overhead and no reliability guarantees — it fires datagrams and forgets them.

---

## Key Characteristics

| Feature | UDP Behaviour |
|---------|--------------|
| **Connection** | Connectionless — no handshake or session state |
| **Reliability** | None — no acknowledgement or retransmission |
| **Ordering** | No sequence numbers — datagrams may arrive out of order |
| **Flow control** | None — sends at application rate regardless of receiver capacity |
| **Header size** | 8 bytes (fixed) |

> [!note] Because UDP lacks these mechanisms, any reliability requirements must be implemented at the **application layer**.

---

## UDP Header Format

| Field | Size | Purpose |
|-------|------|---------|
| Source Port | 16 bits | Sender's application port |
| Destination Port | 16 bits | Target service port |
| Length | 16 bits | Header + data length in bytes |
| Checksum | 16 bits | Optional error detection (IPv4) / Mandatory (IPv6) |

UDP's header is significantly simpler than [[TCP Header Format|TCP's header]] — it carries only what is needed to deliver data to the correct application.

---

## Common UDP Applications

| Application | Why UDP |
|-------------|---------|
| **DNS** | Single request/response; fast; application retries if needed |
| **Video streaming** | Consistent rate > guaranteed delivery; some loss tolerated |
| **VoIP / video calls** | Low latency critical; late audio is useless |
| **Online gaming** | Current state > guaranteed delivery of old state |

See [[TCP vs UDP]] for a full comparison of when to choose each protocol.

---

## Data Reassembly

UDP datagrams are sent over IP, which does not guarantee ordering. Since UDP has **no sequence numbers**, datagrams arriving out of order cannot be reordered by the transport layer — the application receives them in whatever order they arrive.

---

## Related Notes

- [[Week 11a]] — Transport Layer fundamentals, port numbers
- [[Week 12a]] — TCP fundamentals (the reliable alternative)
- [[TCP vs UDP]] — Comparison of transport protocols
- [[TCP vs UDP Traffic Behaviour]] — How UDP and TCP compete under congestion
- [[TCP Header Format]] — TCP's larger header for comparison
