---
tags:
  - networking
  - tcp
  - transport-layer
  - congestion-control
  - week-12
aliases:
  - TCP Congestion Control
  - Slow Start
  - Congestion Avoidance
  - cwnd
  - ssthresh
  - Congestion Window
---

# TCP Congestion Control

> **Source:** [[Week 12b]]

## Two Kinds of Congestion

TCP has to deal with two separate capacity problems:

| Type | Cause | Who Controls It |
|------|-------|-----------------|
| **Receiver congestion** | Receiver's buffer fills up | Receiver advertises `rwnd`; see [[TCP Window Size and Sliding Window]] |
| **Network congestion** | Routers along the path drop packets | Sender manages `cwnd` using congestion control algorithms |

This note focuses on **network congestion** — the sender's side of the problem.

---

## The Congestion Window (cwnd)

The sender maintains a **congestion window** (`cwnd`) — its own estimate of how much data the *network* can handle at once. This is separate from the receiver's window (`rwnd`).

The sender's actual limit is always:
```
bytes in flight ≤ min(cwnd, rwnd)
```

`cwnd` is **not** sent in TCP headers — it is internal state on the sender only. The receiver never sees it directly.

---

## Why Not Just Start Large?

The sender doesn't know the network's capacity when a connection starts. Starting with a large `cwnd` risks immediately overwhelming routers or the receiver:
- Queues fill up
- Packets are dropped
- The whole connection has to slow down anyway

Instead, TCP **probes** for available capacity gradually using two phases: **Slow Start** and **Congestion Avoidance**.

---

## Phase 1 — Slow Start

### Mechanism

- `cwnd` starts at **1 segment** (or a small initial value, currently often 10 in modern systems)
- For **every ACK received**, `cwnd` is incremented by 1
- This means the window **doubles each RTT** — exponential growth

```
RTT 0:  cwnd = 1   → send 1 segment
RTT 1:  cwnd = 2   → send 2 segments  (2 ACKs received → +2)
RTT 2:  cwnd = 4   → send 4 segments  (4 ACKs → +4)
RTT 3:  cwnd = 8
RTT 4:  cwnd = 16
...
```

### Why Is It Called "Slow Start"?

The name is relative to the alternative — sending at full speed immediately. Starting at 1 and doubling is still fast growth. The "slow" refers to the slow, careful initial probe compared to just blasting the full receiver window.

### Slow Start Threshold (ssthresh)

Slow Start continues until `cwnd` reaches `ssthresh`. At that point, TCP switches to Congestion Avoidance to avoid overshooting capacity.

`ssthresh` is initially set to a large value (effectively unlimited), so on a fresh connection TCP keeps doubling until the first loss event, which then sets `ssthresh`.

---

## Phase 2 — Congestion Avoidance

### Mechanism

Once `cwnd ≥ ssthresh`, growth slows to **linear**:
- `cwnd` increases by **1 segment per RTT** (i.e. +1 only after the entire window has been acknowledged)
- One additional ACK no longer triggers an immediate increment — instead, a fractional increment is accumulated

```
cwnd: 8 → 9 → 10 → 11 → 12 → ... (one per RTT)
```

This carefully probes for additional capacity without the explosive growth of Slow Start.

### The Combined Curve

```
cwnd
 ^
20|                                    ●
18|                                ●
16|                    ●───────────
 8|            ●───────
 4|        ●           ← ssthresh crossed; switch to linear
 2|    ●
 1|●
  +────────────────────────────────────> time (RTTs)
   Slow Start ──────┤ Congestion Avoidance
```

---

## Loss Response — What Happens When a Packet Drops

TCP detects loss in two ways:

### 1. Retransmit Timeout (RTO)

A timer expires before the ACK arrives. This indicates serious congestion:

1. `ssthresh = cwnd / 2` (record half the current window as the new threshold)
2. `cwnd = 1` (restart from Slow Start)
3. Timeout period is **extended** (exponential backoff on the timer itself)

This is a harsh reset — transmission rate drops dramatically, relieving network pressure quickly.

### 2. Triple Duplicate ACK (Fast Retransmit / Fast Recovery)

If three duplicate ACKs arrive for the same sequence number, a segment was likely lost but the network isn't severely congested (later segments are getting through):

1. `ssthresh = cwnd / 2`
2. `cwnd = ssthresh` (skip all the way to Congestion Avoidance, not back to 1)
3. Retransmit the missing segment immediately

This is more gentle — the connection recovers faster because it doesn't fall all the way back to Slow Start.

> [!note] The lecture slides cover the timeout-based response. Triple duplicate ACK / Fast Recovery is the modern complement — worth knowing both for completeness.

---

## The Full Lifecycle — Sawtooth Pattern

A long-running TCP connection produces a characteristic sawtooth:

```
cwnd
 ^
 |    /\          /\          /\
 |   /  \        /  \        /  \
 |  /    \      /    \      /    \
 | /      \    /      \    /      \
 |/        \  /        \  /        \
 +──────────\/──────────\/──────────\──> time
             loss        loss        loss
```

Each loss event:
1. Drops the window (exponential backoff)
2. Re-probes with Slow Start up to ssthresh
3. Grows linearly until the next loss

TCP naturally settles at a transmission rate just below the point where the network starts dropping packets.

---

## Adjustable Window — Historical Context

Early TCP implementations tried to find the ideal window by:
- Increasing window size whenever segments were acknowledged
- Fixing the size when the first segment was dropped

This had problems — the "fix on first drop" approach was too crude. The modern Slow Start + Congestion Avoidance + Fast Recovery combination (TCP Reno, TCP Cubic, etc.) is the result of decades of refinement.

> [!note] **TCP Cubic** is the default congestion control algorithm on Linux. It uses a cubic function instead of linear growth during Congestion Avoidance, recovering faster after loss on high-bandwidth long-delay links. **BBR** (Bottleneck Bandwidth and RTT) by Google takes a different approach entirely — modelling the network directly rather than reacting to loss.

---

## Summary of Congestion Control States

| State | Trigger | cwnd Growth | End Condition |
|-------|---------|-------------|---------------|
| **Slow Start** | Connection start or RTO | Exponential (+1 per ACK) | `cwnd ≥ ssthresh` or loss |
| **Congestion Avoidance** | `cwnd ≥ ssthresh` | Linear (+1 per RTT) | Loss event |
| **Fast Recovery** | 3× duplicate ACK | Starts at new ssthresh, linear | New loss or connection ends |
| **RTO Response** | Timeout | Reset to 1, restart Slow Start | — |

---

## Related Notes

- [[TCP Window Size and Sliding Window]] — The receiver window (`rwnd`) side of flow control
- [[TCP vs UDP Traffic Behaviour]] — How UDP's lack of congestion control interacts with TCP
- [[TCP Sequence and Acknowledgement Numbers]] — ACKs drive the cwnd increments
- [[Week 12b]] — Parent note
