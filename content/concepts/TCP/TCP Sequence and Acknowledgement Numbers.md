---
tags:
  - networking
  - tcp
  - transport-layer
  - flow-control
  - week-12
aliases:
  - TCP Sequence Numbers
  - TCP Acknowledgement Numbers
  - Positive Acknowledgement
  - ISN
  - TCP ACK
---

# TCP Sequence and Acknowledgement Numbers

> **Source:** [[Week 12b]]

## Sequence Numbers

### What They Track

TCP sequence numbers track data at the **byte level**, not at the segment level. This is a critical distinction:

- Every byte of data in the stream has a unique sequence number
- A segment's sequence number is the number of its **first byte**
- If a segment carries bytes 1001–2500, its sequence number is **1001**

### Why Byte-Based (Not Segment-Based)?

Counting bytes instead of segments allows TCP to:
- **Retransmit flexibly** — a retransmitted segment can be smaller or larger than the original. The receiver reassembles by byte position regardless of how segments are sized
- **Detect gaps precisely** — the receiver knows exactly which bytes are missing, not just which segment was lost
- **Order correctly** — out-of-order segments are reassembled by sequence number

### Initial Sequence Numbers (ISN)

Each direction of a TCP connection has its own independent sequence space, starting from a randomised ISN:

- Client's ISN is sent in the **SYN**
- Server's ISN is sent in the **SYN-ACK**
- Both are randomised — see [[TCP Three-Way Handshake]] for why

```
Client seq space:  ISN_c, ISN_c+1, ISN_c+2, ...
Server seq space:  ISN_s, ISN_s+1, ISN_s+2, ...
```

---

## Acknowledgement Numbers

### Positive Acknowledgement — The Key Rule

> **ACK number = the next byte the receiver is expecting**

This is **not** the last byte received. It is a forward-looking statement: "I have received everything up to byte N-1; send me byte N next."

### Example

```
Sender sends:   bytes 1–1500   (seq = 1)
Receiver ACKs:  ACK = 1501     ← "send me byte 1501 next"

Sender sends:   bytes 1501–3000 (seq = 1501)
Receiver ACKs:  ACK = 3001     ← "send me byte 3001 next"
```

### ACK Is in Every Segment

TCP carries the acknowledgement number in **every segment**, including those that carry data. This is called **piggybacking**:

```
Server sends data (seq = 5000, len = 500)
  AND acknowledges client data: ACK = 2800

One segment carries both application data and an ACK for the other direction.
```

### No ACK = Loss Signal

TCP uses the **absence of an ACK** as its signal that data was lost:
- Sender starts a **retransmit timer** when it sends a segment
- If the timer expires before an ACK arrives, the segment is retransmitted
- This is distinct from UDP, which has no retransmission at all

---

## Cumulative vs Selective Acknowledgement

### Cumulative ACK (standard)

The ACK number acknowledges **all bytes up to that point** — it is cumulative. If bytes 1–1000 and 1501–2000 arrive but 1001–1500 are missing:

```
Receiver ACKs: 1001   (stuck — won't advance past the gap)
```

The sender must retransmit from 1001, even though 1501–2000 already arrived.

### Selective ACK (SACK — extension)

With **SACK** (RFC 2018), the receiver can report non-contiguous received ranges:

```
ACK = 1001, SACK = [1501–2000]
```

The sender then only retransmits the missing 1001–1500, not everything from 1001 onward. SACK is widely supported but is negotiated during the handshake.

---

## FIN and SYN Consume Sequence Numbers

Although FIN and SYN carry no data payload, they each consume **one sequence number**. This ensures the peer can ACK them specifically:

```
Client sends FIN (seq = 5000)
Server ACKs:  ACK = 5001   ← FIN consumed seq 5000
```

---

## Common Exam Trap

> [!warning] **"The ACK number is the last byte received"** — this is wrong. It is always the **next byte expected**. If the receiver just got byte 3000, the ACK = **3001**, not 3000.

---

## Related Notes

- [[TCP Three-Way Handshake]] — Where ISNs are established
- [[TCP Window Size and Sliding Window]] — How sequence numbers interact with the window
- [[TCP Connection Termination]] — How FIN consumes a sequence number
- [[Week 12b]] — Parent note
