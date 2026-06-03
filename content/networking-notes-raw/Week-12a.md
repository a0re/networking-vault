---
tags:
  - networking
  - tcp
  - transport-layer
  - week-12
week: 12
aliases:
  - Week 12a
  - TCP Fundamentals
  - TCP Introduction
---

# Week 12a — TCP: Transmission Control Protocol

> **Source:** Lecture 12a — TNE10006/TNE60006 Networks and Switching  
> **RFC:** 793

## Overview

TCP is the **reliable, connection-oriented** transport layer (Layer 4) protocol used in TCP/IP networks. It sits above IP (which is best-effort and unreliable) and provides the guarantees that IP does not.

---

## What TCP Provides

| Feature | What It Means |
|---------|--------------|
| **Connection-oriented** | A session is established before any data is sent |
| **Reliable delivery** | Lost segments are detected and retransmitted |
| **Ordered delivery** | Data arrives at the application in the same order it was sent |
| **Flow control** | Sender rate is regulated to match what the receiver and network can handle |
| **Acknowledgement** | Receiver confirms receipt; silence signals loss |

> [!note] All these features come at a cost — TCP has a larger, more complex header than UDP and introduces connection setup overhead. It is chosen when correctness matters more than raw speed.

---

## TCP Is Connection-Oriented

Before sending data, TCP establishes a **session** via the three-way handshake. This setup phase:

- Confirms the destination device is reachable
- Confirms the target port has a service listening and ready to reply
- Negotiates parameters — including flow control and initial sequence numbers

See → [[TCP Three-Way Handshake]]

---

## Reliable Delivery

TCP tracks every segment it sends. If an acknowledgement is not received within a timeout period, the segment is assumed lost and **retransmitted**.

This is distinct from UDP, which fires and forgets — if a UDP packet is lost, the application layer must handle it (or accept the loss).

See → [[TCP Sequence and Acknowledgement Numbers]]

---

## Ordered Delivery

IP is a best-effort protocol — packets can arrive **out of order**, take different routes, and be delayed by varying amounts. TCP handles this transparently:

- Every segment carries a **sequence number** that identifies its position in the byte stream
- The receiver buffers out-of-order segments
- The transport layer **reassembles** the stream in the correct order before passing it to the application

The application always sees data in the order it was sent, regardless of how IP routed the packets.

See → [[TCP Sequence and Acknowledgement Numbers]]

---

## Flow Control

TCP monitors the state of the connection — including buffer capacity at the receiver and congestion signals from the network — and **slows the sending rate** when needed. This prevents overwhelming the receiver or the network.

See → [[TCP Window Size and Sliding Window]]  
See → [[TCP Congestion Control]]

---

## TCP Header

TCP has a significantly **larger and more complex header than UDP** because it carries all the information needed to support its features.

See → [[TCP Header Format]]

---

## TCP Segment Flow — Client/Server

The general flow of a TCP data transfer mirrors UDP's client/server model but adds reliability:

1. Data stream from the application is **divided into segments** at the transport layer
2. Each segment is given a **sequence number**
3. Segments may arrive at the receiver **out of order** (IP does not guarantee ordering)
4. The receiver's transport layer **reorders** segments using their sequence numbers
5. The reassembled stream is delivered to the application layer in the correct order

---

## Acknowledgements — Positive ACK

TCP uses **positive acknowledgement**: the receiver always reports the **next byte it expects**, not the last byte it received.

```
Sender sends bytes 1–1000   → Receiver replies: ACK 1001
Sender sends bytes 1001–2000 → Receiver replies: ACK 2001
```

If the sender does not receive an ACK within its timeout window, it retransmits the unacknowledged segment.

> [!important] Sequence and ACK numbers are tracked in **bytes**, not segments. This allows flexibility in retransmission sizing and precise gap detection.

See → [[TCP Sequence and Acknowledgement Numbers]]

---

## Week 12 Notes

| Note | Content |
|------|---------|
| [[Week 12b]] | Flow control in depth — handshake, window size, congestion control, TCP vs UDP |

## Related Concept Notes

- [[TCP Header Format]] — Fields and their purpose
- [[TCP Three-Way Handshake]] — Session establishment
- [[TCP Sequence and Acknowledgement Numbers]] — How TCP tracks every byte
- [[TCP Window Size and Sliding Window]] — Flow control mechanics
- [[TCP Congestion Control]] — Slow Start, Congestion Avoidance
- [[TCP vs UDP Traffic Behaviour]] — Protocol comparison under congestion
