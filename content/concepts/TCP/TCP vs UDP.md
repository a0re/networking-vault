---
tags:
  - networking
  - tcp
  - udp
  - transport-layer
  - week-12
aliases:
  - TCP vs UDP
  - TCP and UDP Comparison
  - Transport Layer Protocols
  - When to Use TCP
  - When to Use UDP
---

# TCP vs UDP

> **Source:** [[Week 12a]]

## The Transport Layer's Two Protocols

IP is best-effort — it delivers packets on a best-effort basis with no guarantees of delivery, ordering, or timing. The transport layer sits above IP and offers two very different approaches to working with it:

| | **TCP** | **UDP** |
|-|---------|---------|
| **Full name** | Transmission Control Protocol | User Datagram Protocol |
| **RFC** | 793 | 768 |
| **Connection** | Connection-oriented | Connectionless |
| **Reliability** | Guaranteed delivery via retransmission | Best-effort; no retransmission |
| **Ordering** | Guaranteed in-order delivery | No ordering guarantee |
| **Flow control** | Yes (window size, cwnd) | No |
| **Congestion control** | Yes (Slow Start, Congestion Avoidance) | No |
| **Header size** | 20–60 bytes | 8 bytes (fixed) |
| **Speed** | Slower (overhead and setup) | Faster (low overhead, no handshake) |
| **When to use** | Correctness required | Speed or latency more important than completeness |

---

## TCP — Features in Detail

### Connection-Oriented

TCP establishes a session via the [[TCP Three-Way Handshake]] before any data is sent. This verifies:
- The destination is reachable
- The target port has a service listening
- Both sides have negotiated initial parameters (sequence numbers, window sizes, options like MSS and SACK)

### Reliable Delivery

Every sent segment is tracked. If an ACK is not received within the retransmit timeout (RTO), the segment is resent. See [[TCP Sequence and Acknowledgement Numbers]].

### Ordered Delivery

TCP assigns a **sequence number** to every byte. Out-of-order segments are buffered and reordered at the receiver's transport layer before the application sees them.

### Flow and Congestion Control

TCP has two complementary mechanisms:
- **Receiver flow control** — the receiver advertises how much buffer space it has (`rwnd`). See [[TCP Window Size and Sliding Window]].
- **Congestion control** — the sender probes network capacity using Slow Start and Congestion Avoidance (`cwnd`). See [[TCP Congestion Control]].

---

## UDP — Features in Detail

### Connectionless

UDP sends datagrams with no handshake and no session state. Each datagram is independent.

### No Reliability

UDP provides no acknowledgement, no retransmission, and no detection of lost datagrams. If a packet is lost, UDP doesn't know and doesn't care. Any reliability required must be implemented by the **application layer**.

### No Ordering

UDP datagrams may arrive out of order. The application receives them as they arrive.

### No Flow or Congestion Control

UDP transmits at whatever rate the application specifies. It will not slow down in response to dropped packets or receiver buffer pressure. See [[TCP vs UDP Traffic Behaviour]] for what happens when UDP and TCP compete for bandwidth.

### Fixed 8-Byte Header

| Field | Size | Purpose |
|-------|------|---------|
| Source Port | 16 bits | Sender's port |
| Destination Port | 16 bits | Target service port |
| Length | 16 bits | Length of UDP header + data in bytes |
| Checksum | 16 bits | Error detection (optional in IPv4, mandatory in IPv6) |

---

## Choosing Between TCP and UDP

The right choice depends on what the application needs:

### Use TCP When:

- Data must arrive completely and correctly (file transfers, web pages, email, database queries)
- Order matters and the application shouldn't have to reassemble (HTTP, FTP, SSH, SMTP)
- The application has no tolerance for missing data

### Use UDP When:

- **Low latency is critical** — a retransmitted packet arrives too late to be useful
- **Some loss is acceptable** — a dropped video frame or game state update is better than a delayed one
- **Small, self-contained transactions** — a DNS query fits in one packet; TCP's handshake overhead isn't worth it
- **Multicast or broadcast** — TCP is point-to-point only; UDP supports one-to-many

### Common Protocol Choices

| Application | Protocol | Reason |
|-------------|----------|--------|
| Web (HTTP/HTTPS) | TCP | Page must be complete and correct |
| File transfer (FTP, SFTP) | TCP | Every byte must arrive |
| Email (SMTP, IMAP) | TCP | Message integrity required |
| SSH / remote shell | TCP | Commands must arrive in order |
| DNS | UDP | Single request/response; fast; application retries |
| VoIP / video calls | UDP | Late audio is useless; some loss tolerated |
| Online gaming | UDP | Current state > guaranteed delivery of old state |
| Live video streaming | UDP | Consistent rate matters more than every frame |
| HTTP/3 (QUIC) | UDP* | QUIC adds its own reliability/CC on top of UDP |

> [!note] QUIC (used in HTTP/3) is notable — it implements connection-oriented behaviour, reliability, ordering, and congestion control on top of UDP. This gives it TCP's guarantees while avoiding TCP's head-of-line blocking and allowing connection migration.

---

## The Header Size Trade-off in Practice

On a small DNS query (say, 30 bytes of payload):

```
TCP: 20-byte IP header + 20-byte TCP header + 30-byte payload = 70 bytes total
     Plus: 3 handshake segments before data even starts

UDP: 20-byte IP header + 8-byte UDP header + 30-byte payload = 58 bytes total
     No setup — packet fires immediately
```

For a 1 GB file transfer, TCP's per-segment overhead becomes negligible relative to the data. For millions of tiny transactions per second (DNS, NTP, DHCP), UDP's simplicity is a significant win.

---

## Both Use Ports

Despite their differences, both TCP and UDP use **port numbers** to multiplex multiple services on the same host:

- Port numbers are 16-bit values (0–65535)
- Well-known ports (0–1023) are assigned by IANA (e.g. 80 = HTTP, 443 = HTTPS, 53 = DNS)
- Ephemeral ports (typically 49152–65535) are assigned by the OS for client-side connections

---

## Related Notes

- [[TCP Header Format]] — Full breakdown of TCP's header fields
- [[TCP vs UDP Traffic Behaviour]] — What happens when they share a congested link
- [[TCP Congestion Control]] — Why TCP backs off and UDP does not
- [[Week 12a]] — Parent note
