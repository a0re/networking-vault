---
tags:
  - networking
  - tcp
  - transport-layer
  - connection-management
  - week-12
aliases:
  - TCP Termination
  - FIN-ACK
  - TCP Connection Teardown
  - Four-Way Handshake
---

# TCP Connection Termination

> **Source:** [[Week 12b]]

## Overview

Unlike the three-way handshake that opens a connection, TCP uses a **four-step sequence** to close one. This is because TCP is **full-duplex** — each direction of data flow is independent and must be closed separately.

---

## The FIN-ACK Sequence

```
A                              B
|                               |
|-------- FIN ----------------->|   (1) A signals: done sending
|                               |
|<-------- ACK -----------------|   (2) B acknowledges A's FIN
|                               |
|         [B may still send]    |   ← half-close state
|                               |
|<-------- FIN -----------------|   (3) B signals: done sending
|                               |
|-------- ACK ----------------->|   (4) A acknowledges B's FIN
|                               |
         Connection closed
```

### Step-by-Step

| Step | Who | Flag | Meaning |
|------|-----|------|---------|
| 1 | A → B | **FIN** | A has finished sending data |
| 2 | B → A | **ACK** | B acknowledges; B may still send |
| 3 | B → A | **FIN** | B has also finished sending data |
| 4 | A → B | **ACK** | A acknowledges; connection fully closed |

---

## Half-Close State

Between steps 2 and 3, the connection is **half-closed**:
- A has finished sending, but B has not yet
- B can continue transmitting data to A
- A must still receive and ACK this data
- Only when B sends its own FIN (step 3) does the B→A direction close

This matters in practice for protocols where the server sends a large response after the client signals it is done (e.g. a server finishing writing a large file transfer after the client closes its upload).

---

## Why Four Steps, Not Three?

The opening handshake combines SYN and ACK in one segment (SYN-ACK) because both sides need to synchronise at the same time. Closing is different — each endpoint independently decides when it has no more data to send, so the two FINs cannot generally be combined.

> [!note] In some implementations, steps 2 and 3 (B's ACK and B's FIN) may be combined into a single **FIN-ACK** segment if B has no more data to send immediately. This reduces the teardown to three segments, similar to the handshake — but conceptually it is still a four-step logical process.

---

## TIME_WAIT State

After step 4, A does **not** immediately free the connection. It enters a **TIME_WAIT** state for `2 × MSL` (Maximum Segment Lifetime, typically 60–120 seconds). This ensures:

1. The final ACK (step 4) reaches B — if it is lost, B will retransmit its FIN and A can re-ACK
2. Any stale packets from this connection expire before a new connection on the same port pair is allowed

> [!warning] TIME_WAIT can cause issues on high-throughput servers that cycle through many short-lived connections — all those sockets sitting in TIME_WAIT consume resources. This is why server tuning sometimes adjusts `SO_REUSEADDR` or `tcp_tw_reuse`.

---

## RST — Abrupt Termination

The FIN-ACK sequence is a **graceful** close. TCP also supports an abrupt close via the **RST (Reset)** flag:

| Situation | Behaviour |
|-----------|-----------|
| Application crashes | OS sends RST to peer |
| Connection to closed port | Target sends RST immediately |
| Firewall rejects connection | May send RST to client |

RST terminates both directions immediately — no four-step process, no TIME_WAIT.

---

## Related Notes

- [[TCP Three-Way Handshake]] — How the connection is opened
- [[TCP Sequence and Acknowledgement Numbers]] — How FIN consumes a sequence number
- [[Week 11a]] — Transport Layer fundamentals, port numbers
- [[Week 12b]] — Parent note
