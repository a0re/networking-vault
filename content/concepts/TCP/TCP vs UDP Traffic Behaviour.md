---
tags:
  - networking
  - tcp
  - udp
  - transport-layer
  - congestion-control
  - week-12
aliases:
  - TCP vs UDP Traffic Behaviour
  - UDP Congestion
  - TCP Fairness
  - Traffic Interaction
---

# TCP vs UDP Traffic Behaviour

> **Source:** [[Week 12b]]

## TCP Flows in a Congested Network

TCP is designed to **behave fairly** under congestion. Multiple TCP flows sharing a bottleneck link will:

- Each detect congestion (via dropped packets / timeouts)
- Each back off independently
- Converge on roughly equal shares of available bandwidth

This property is called **TCP friendliness** or **TCP fairness** — a TCP flow won't aggressively crowd out other TCP flows.

See [[TCP Congestion Control]] for the mechanisms that make this work (Slow Start, Congestion Avoidance, backoff on loss).

---

## UDP Has No Congestion Control

UDP is a **connectionless, best-effort** protocol. It has no:
- Sequence numbers (in the sense TCP uses them)
- Acknowledgements
- Retransmission
- Window management
- Congestion window

A UDP sender transmits at whatever rate the application dictates — it does not back off in response to dropped packets.

---

## What Happens When TCP and UDP Share a Link

### UDP Floods the Link

```
Available bandwidth: 100 Mbps
UDP sender: transmits at 80 Mbps (constant)
TCP sender: starts at full speed, detects drops, backs off

Result:
  UDP: 80 Mbps  (unchanged)
  TCP: 20 Mbps  (or less — keeps backing off as UDP holds its rate)
```

TCP backs off; UDP does not. UDP effectively **starves TCP** on a congested link. The TCP flow suffers disproportionately.

### Two TCP Flows Sharing the Link

```
Available bandwidth: 100 Mbps
TCP flow A: starts high, detects drops, backs off
TCP flow B: starts high, detects drops, backs off

Result (at equilibrium):
  TCP A: ~50 Mbps
  TCP B: ~50 Mbps
```

Both flows back off proportionally. Bandwidth is shared roughly fairly.

### TCP Backs Off, More Room for UDP

When TCP reduces its rate in response to congestion, the freed bandwidth doesn't go to waste — it becomes available for other flows. UDP traffic will naturally fill any available headroom since it transmits at whatever rate the application provides. However, UDP doesn't "give back" — if TCP later tries to reclaim bandwidth, it must compete with UDP that won't yield.

---

## Summary of Interactions

| Scenario | TCP Behaviour | UDP Behaviour | Outcome |
|----------|--------------|---------------|---------|
| UDP floods congested link | Backs off aggressively | Holds rate | **TCP starved; UDP wins** |
| TCP vs TCP on congested link | Both back off | — | **Bandwidth shared fairly** |
| TCP backs off (congestion) | Reduces rate | Fills freed headroom | **UDP absorbs TCP's slack** |
| Network returns to normal | TCP grows window again | UDP unaffected | **TCP slowly reclaims share** |

---

## Applications That Use UDP — and Why

UDP is chosen specifically because its lack of overhead and backoff makes it suitable for:

| Use Case | Why UDP | Trade-off |
|----------|---------|-----------|
| **VoIP / video calls** | Low latency critical; a late packet is useless | Some packet loss is acceptable |
| **Online gaming** | State updates must be current; retransmitting stale input is wasteful | Occasional lost updates tolerated |
| **Live video streaming** | Consistent rate matters more than guaranteed delivery | Buffering or artefacts on loss |
| **DNS queries** | Single small request/response; connection overhead not worth it | Application retries if needed |

---

## The Problem: Congestion Collapse

If a large proportion of traffic on a link is UDP (or otherwise not congestion-controlled), the network can enter **congestion collapse**:
- Queues fill at routers
- Routers drop packets indiscriminately
- TCP flows back off severely
- UDP maintains rate — contributing to continued congestion
- Throughput plummets despite the link being saturated

This was a real problem in the early internet (1986 congestion collapse on ARPANET) and motivated the development of TCP's congestion control algorithms.

---

## Solutions — Congestion-Aware UDP

Modern protocols add congestion awareness on top of UDP at the application or protocol layer:

| Protocol | Layer | Approach |
|----------|-------|----------|
| **QUIC** (HTTP/3) | Application | Implements its own congestion control (similar to TCP Cubic/BBR) over UDP |
| **DCCP** | Transport | Datagram Congestion Control Protocol — UDP-like but with negotiated CC |
| **WebRTC** | Application | Uses GCC (Google Congestion Control) for real-time media |
| **SCTP** | Transport | Stream Control Transmission Protocol — congestion control + multi-streaming |

> [!note] QUIC is the most relevant modern example. HTTP/3 runs over QUIC, which gives it TCP-like reliability and congestion control while avoiding TCP's head-of-line blocking problem.

---

## Ongoing Research

The lecture notes that there is **continuous work** to define new Congestion Control Schemes with the goal of improving overall throughput without violating TCP fairness — i.e. a new algorithm shouldn't be able to crowd out standard TCP flows.

Notable modern algorithms:
- **TCP Cubic** — default on Linux; faster recovery on high-bandwidth links
- **BBR (Bottleneck Bandwidth and RTT)** — Google's model-based approach; doesn't rely on packet loss as the congestion signal
- **LEDBAT** — used by BitTorrent; designed to be lower priority than standard TCP (yields to other traffic)

---

## Related Notes

- [[TCP Congestion Control]] — The mechanisms TCP uses to back off
- [[TCP Window Size and Sliding Window]] — Flow control from the receiver's perspective
- [[Week 11b]] — UDP protocol (no congestion control)
- [[Week 12b]] — Parent note
