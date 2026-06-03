---
tags:
  - networking
  - tcp
  - transport-layer
  - flow-control
  - week-12
aliases:
  - TCP Window Size
  - Sliding Window
  - Receive Window
  - rwnd
  - TCP Flow Control
---

# TCP Window Size and Sliding Window

> **Source:** [[Week 12b]]

## The Problem Window Size Solves

Without a window, TCP would work like a walkie-talkie — send one segment, wait for an ACK, send the next. On any link with non-trivial latency, the sender spends most of its time idle waiting for ACKs. This wastes bandwidth.

The **window** allows the sender to have multiple segments in-flight simultaneously — sent but not yet acknowledged — filling the network pipe efficiently.

---

## How Window Size Works

The **window size** is the maximum number of bytes that can be in-flight at once. It is advertised by the **receiver** based on how much buffer space it has available.

```
Window size = 3000 bytes (2 × 1500-byte segments)

Sender                              Receiver
  |                                   |
  |--- seq 1    [1500 bytes] -------->|  receives bytes 1–1500
  |--- seq 1501 [1500 bytes] -------->|  receives bytes 1501–3000
  |                  ← window full    |
  |<--- ACK 3001 ---------------------|  window fulfilled; slide forward
  |                                   |
  |--- seq 3001 [1500 bytes] -------->|  next window begins
  |--- seq 4501 [1500 bytes] -------->|
  |<--- ACK 6001 ---------------------|
```

The ACK number tells the sender where to **slide** the window forward. All bytes before the ACK number have been received; the sender can now transmit the next window's worth of data.

---

## The Window Is a Constraint, Not a Mandate

The sender transmits **up to** the window size — it may send less if:
- The application hasn't produced enough data yet
- The congestion window is smaller (see [[TCP Congestion Control]])
- Nagle's algorithm is batching small writes

The effective window is always:
```
Effective Window = min(receiver window rwnd, congestion window cwnd)
```

---

## Unpredictable Transfer Times

The sliding window introduces timing behaviour that isn't always intuitive:

### All Data in the Window Is Immediately Available

The sender doesn't wait for individual ACKs within a window — it sends the whole window back-to-back. This is what makes windowing efficient.

### But Transfer Isn't Always Immediate

When an application passes data to TCP:
1. Data goes into the **TCP send buffer** (a queue in the OS kernel)
2. The TCP stack **segments** the data (decides how to carve it into segments)
3. The stack decides **when** to send based on window availability and Nagle's algorithm

The application cannot assume data hits the wire the moment it calls `send()`.

### Retransmission Delays

If a segment in the middle of a window is lost:
- The sender must **wait for a timeout** before retransmitting (unless SACK is in use)
- Other segments that were already sent but arrive after the gap cannot be delivered to the application until the gap is filled (head-of-line blocking)
- Segments beyond the gap must wait in the receiver's buffer

---

## Window Size: Too Small vs Too Large

| Window Size | Effect | Why It Happens |
|-------------|--------|----------------|
| **Too small** | Bandwidth underutilised | Sender keeps stalling to wait for ACKs; network pipe is mostly empty |
| **Too large** | Queues form, packets drop | Sender overwhelms the receiver's buffer or intermediate routers |
| **Ideal** | Full pipe utilisation | Enough data in-flight to keep the link busy for exactly one RTT |

### Bandwidth-Delay Product

The ideal window size fills the entire pipe:

```
Ideal window = Bandwidth × Round Trip Time (RTT)
```

Example: 100 Mbps link, 50 ms RTT
```
Ideal window = 100,000,000 bits/s × 0.05 s = 5,000,000 bits = ~625 KB
```

A window smaller than this leaves the link idle part of the time.

---

## Receiver Window Advertisement

The receiver tells the sender its current window size in the **Window Size field** of every TCP segment it sends back. This is dynamic — as the receiver's application reads data from its buffer, it can advertise a larger window; if it falls behind processing, it shrinks the window.

### Zero Window

If the receiver's buffer is completely full, it advertises a **window size of 0**, which pauses the sender entirely. The sender then sends periodic **window probe** segments to check if the receiver has freed space.

---

## Relationship to Congestion Control

The receiver window (`rwnd`) controls the **receiver's** capacity. But the network itself has limits too — this is handled by the **congestion window** (`cwnd`), maintained by the sender. See [[TCP Congestion Control]] for how `cwnd` is managed.

The sender always uses the **smaller** of the two:
```
bytes_in_flight ≤ min(rwnd, cwnd)
```

---

## Related Notes

- [[TCP Sequence and Acknowledgement Numbers]] — Sequence numbers drive the window mechanics
- [[TCP Congestion Control]] — The other half of window management (cwnd)
- [[TCP Three-Way Handshake]] — Initial window size is advertised in the SYN
- [[Week 11a]] — Transport Layer fundamentals, port numbers
- [[Week 12b]] — Parent note
