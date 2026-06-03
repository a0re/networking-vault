---
tags:
  - networking
  - tcp
  - transport-layer
  - flow-control
  - congestion-control
  - week-12
week: 12
aliases:
  - Week 12b
  - TCP Flow Control
---

# Week 12b — TCP Flow Control

> **Source:** Lecture 12b — TNE10006/TNE60006 Networks and Switching

## Overview

This lecture covers how TCP manages data transmission reliably and efficiently between two endpoints. It builds on the transport layer fundamentals and examines three interconnected concerns:

1. **Connection management** — how TCP establishes and tears down sessions
2. **Flow control** — preventing the sender from overwhelming the receiver
3. **Congestion control** — preventing the sender from overwhelming the network

---

## TCP Connection Establishment

See → [[TCP Three-Way Handshake]]

TCP is **connection-oriented** — before any data is transferred, both endpoints must agree to communicate and synchronise state. This is done via the **three-way handshake**:

| Step | Sender → Receiver | Purpose |
|------|-------------------|---------|
| **SYN** | Client → Server | Client initiates; sends randomised Initial Sequence Number (ISN) |
| **SYN-ACK** | Server → Client | Server acknowledges ISN, sends its own ISN |
| **ACK** | Client → Server | Client acknowledges server ISN; connection open |

> [!note] ISNs are randomised (not starting at 0) to prevent stale packets from old connections being accepted as valid in new sessions.

The handshake also verifies:
- The destination is reachable
- The target port has an active service listening

---

## TCP Connection Termination

See → [[TCP Connection Termination]]

Closing a TCP connection is a **four-step** process because each direction closes independently:

```
A                    B
|---- FIN ---------->|   A is done sending
|<--- ACK -----------|   B acknowledges
|<--- FIN -----------|   B is done sending
|---- ACK ---------->|   A acknowledges → connection closed
```

Between steps 2 and 3, B may still send data — TCP supports **half-open** connections.

---

## Sequence & Acknowledgement Numbers

See → [[TCP Sequence and Acknowledgement Numbers]]

### Sequence Numbers

- Counted in **bytes**, not segments
- Allows retransmitted segments to be resized (larger or smaller than the original)
- Initial values exchanged during the handshake

### Acknowledgement Numbers (Positive ACK)

TCP uses **positive acknowledgement** — the ACK field contains the **next byte expected**, not the last byte received:

| Behaviour | Example |
|-----------|---------|
| Receiver gets bytes 1–1500 | ACK = **1501** |
| Receiver gets bytes 1–3000 | ACK = **3001** |
| ACK piggybacked on data | Every TCP segment carries an ACK |
| No ACK received = loss | Sender retransmits after timeout |

---

## Flow Control — Window Size

See → [[TCP Window Size and Sliding Window]]

The **window size** limits how many bytes can be in-flight (sent, unacknowledged) at once:

```
Window size = 3000 bytes

Sender                         Receiver
 |-- seq 1    [1500B] -------->|
 |-- seq 1501 [1500B] -------->|   window filled
 |<-- ACK 3001 ----------------|   window slides forward
 |-- seq 3001 [1500B] -------->|
 ...
```

**Window size too small** → bandwidth wasted waiting for ACKs
**Window size too large** → queues form, packets drop

The **effective/sliding window** the sender uses at any moment is:

```
Effective Window = min(receiver window, congestion window)
```

---

## Congestion Control

See → [[TCP Congestion Control]]

TCP distinguishes two separate congestion problems:

| Type | Problem | Mechanism |
|------|---------|-----------|
| **Receiver congestion** | Receiver's buffer fills up | Receiver advertises `rwnd` (receive window) |
| **Network congestion** | Routers along the path are dropping packets | Sender maintains `cwnd` (congestion window) |

### Slow Start

- `cwnd` begins at **1 segment**
- Each ACK received increments `cwnd` by 1 → **exponential growth**
- Rapidly probes available bandwidth from a safe starting point

### Congestion Avoidance

- Once `cwnd` reaches `ssthresh` (slow start threshold), growth slows to **linear** (+1 per full window ACK'd)
- Carefully approaches network capacity without overshooting

### On Loss (Timeout)

- `cwnd` is **halved**
- Timeout wait period is **extended** (exponential backoff)
- TCP re-enters Slow Start from the new lower threshold
- Effect: transmission rate drops sharply, relieving congestion quickly

```
cwnd
 ^
 |                          /
 |                        /    ← linear (congestion avoidance)
 |                   ●--/
 |              ●--/
 |         ●--/               ← threshold crossed
 |    ●--/
 |●/                          ← slow start (exponential)
 +------------------------------> time
```

---

## TCP vs UDP — Traffic Interaction

See → [[TCP vs UDP Traffic Behaviour]]

TCP flows self-regulate — they back off when congestion is detected and share bandwidth fairly with other TCP flows.

UDP has **no congestion control**:

| Scenario | Outcome |
|----------|---------|
| UDP floods a congested link | UDP holds rate; TCP backs off — TCP is starved |
| TCP vs TCP | Both back off; bandwidth shared roughly equally |
| TCP reduces rate | More headroom for UDP, but UDP doesn't reciprocate |

> [!warning] UDP-based applications (VoIP, gaming, video streaming) should implement their own rate limiting, or use a congestion-aware protocol like **QUIC**.

---

## Key Formulas & Values to Remember

| Concept | Value / Formula |
|---------|----------------|
| Effective window | `min(rwnd, cwnd)` |
| Slow Start growth | Exponential (doubles per RTT) |
| Congestion Avoidance growth | Linear (+1 per window fully ACK'd) |
| On timeout | `cwnd = cwnd / 2`; reset to Slow Start |
| ACK number meaning | Next expected byte (not last received) |
| ISN | Randomised; exchanged in SYN / SYN-ACK |

---

## Related Notes

- [[TCP Three-Way Handshake]] — Full detail on connection setup
- [[TCP Connection Termination]] — FIN-ACK four-step teardown
- [[TCP Sequence and Acknowledgement Numbers]] — Byte-level tracking and positive ACK
- [[TCP Window Size and Sliding Window]] — Window mechanics and transfer timing
- [[TCP Congestion Control]] — Slow Start, Congestion Avoidance, timeout response
- [[TCP vs UDP Traffic Behaviour]] — How the two protocols interact under congestion
- [[Wireless Networking]] — CSMA/CA: the Layer 2 collision avoidance analogue
- [[Lecture-02b]] — Ethernet, CSMA/CD, MAC addressing
