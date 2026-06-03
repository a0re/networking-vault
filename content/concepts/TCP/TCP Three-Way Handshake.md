---
tags:
  - networking
  - tcp
  - transport-layer
  - connection-management
  - week-12
aliases:
  - Three Way Handshake
  - TCP Handshake
  - SYN SYN-ACK ACK
  - TCP Connection Setup
---

# TCP Three-Way Handshake

> **Source:** [[Week 12b]]

## Purpose

Before TCP transfers any data, both endpoints must establish a shared connection state. The handshake achieves three things simultaneously:

| Goal | How |
|------|-----|
| **Reachability** | Confirms the destination device is online and the target port has a service listening |
| **ISN exchange** | Both sides agree on starting sequence numbers for the session |
| **Pre-data guarantee** | No application data is sent until the handshake completes |

---

## The Three Steps

```
Client                         Server
  |                               |
  |-------- SYN (seq=X) -------->|   Step 1: Client initiates
  |                               |
  |<--- SYN-ACK (seq=Y, ack=X+1)-|   Step 2: Server responds
  |                               |
  |-------- ACK (ack=Y+1) ------>|   Step 3: Client confirms
  |                               |
  |======= DATA TRANSFER ========|
```

### Step 1 — SYN

- Client sets the **SYN flag**
- Sends a randomised **Initial Sequence Number (ISN)** — call it `X`
- Chooses a random ephemeral source port (e.g. 1061)
- Targets the server's well-known port (e.g. 80 for HTTP)

### Step 2 — SYN-ACK

- Server sets both the **SYN** and **ACK** flags
- Acknowledges the client's ISN: `ACK = X + 1` (next byte expected from client)
- Sends its own randomised ISN: `seq = Y`
- Source port = server service port (80); destination = client ephemeral port (1061)

### Step 3 — ACK

- Client sets the **ACK** flag
- Acknowledges server's ISN: `ACK = Y + 1`
- Connection is now **ESTABLISHED** on both sides
- Data transfer can begin

---

## Why Are ISNs Randomised?

Initial Sequence Numbers are **never set to 0**. Two reasons:

**1. Stale packet prevention**
If a connection is closed and reopened quickly, old packets from the previous session still wandering the network could have sequence numbers that fall inside the new connection's window. A randomised ISN makes this statistically impossible.

**2. Security**
Predictable ISNs are vulnerable to **TCP session hijacking** — an attacker who can guess the next ISN can inject forged segments into an existing connection. Randomisation prevents this.

---

## What Wireshark Shows

From the lecture slides (Wireshark capture of a real HTTP handshake):

**Frame 10 — SYN (Client → Server)**
- SYN flag set
- Sequence number: 0 (relative value; actual ISN is randomised by OS)
- Source port: 1061 (ephemeral)
- Destination port: 80 (HTTP)

**Frame 11 — SYN-ACK (Server → Client)**
- ACK flag set (acknowledges client ISN + 1)
- SYN flag set (server's own ISN for the server→client direction)
- Source port: 80; Destination port: 1061

**Frame 12 — ACK (Client → Server)**
- ACK flag set
- Acknowledges server ISN + 1
- No data payload — pure handshake segment

---

## Common Misconceptions

> [!warning] **"The connection is established after SYN-ACK"** — Not quite. The server considers it half-open after sending SYN-ACK. The connection is fully established only after the server receives the final ACK. This distinction matters for **SYN flood attacks**, where attackers send many SYNs without completing the handshake, exhausting server resources.

> [!note] The handshake is **bidirectional** — each direction has its own ISN and sequence space. This is why SYN-ACK carries both a SYN (server's ISN) and an ACK (acknowledging client's ISN).

---

## Related Notes

- [[TCP Connection Termination]] — How the connection is gracefully closed
- [[TCP Sequence and Acknowledgement Numbers]] — What the ISNs are used for during data transfer
- [[Week 12b]] — Parent note
