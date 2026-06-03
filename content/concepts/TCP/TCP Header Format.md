---
tags:
  - networking
  - tcp
  - transport-layer
  - week-12
aliases:
  - TCP Header
  - TCP Header Format
  - TCP Segment Structure
---

# TCP Header Format

> **Source:** [[Week 12a]]

## Overview

TCP has a **larger, more complex header than UDP** because it must carry all the information needed to support reliable, ordered, flow-controlled delivery. The minimum TCP header is **20 bytes** (without options), compared to UDP's fixed 8 bytes.

Every field exists to serve one or more of TCP's core features.

---

## Header Fields

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |           |U|A|P|R|S|F|                               |
| Offset| Reserved  |R|C|S|S|Y|I|            Window            |
|       |           |G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (if Data Offset > 5)               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             Data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

## Field Reference

### Addressing

| Field | Size | Purpose |
|-------|------|---------|
| **Source Port** | 16 bits | Port number of the sending process |
| **Destination Port** | 16 bits | Port number of the receiving process; identifies the target service (e.g. 80 = HTTP, 443 = HTTPS) |

Together, source IP + source port + destination IP + destination port form a **4-tuple** that uniquely identifies a TCP connection (socket pair).

---

### Reliability Fields

| Field | Size | Purpose |
|-------|------|---------|
| **Sequence Number** | 32 bits | Position of the first byte of this segment in the sender's byte stream; used for ordering and gap detection |
| **Acknowledgement Number** | 32 bits | Next byte the receiver expects from the sender; valid only when the ACK flag is set |

See → [[TCP Sequence and Acknowledgement Numbers]]

---

### Header Structure

| Field | Size | Purpose |
|-------|------|---------|
| **Data Offset** | 4 bits | Length of the TCP header in 32-bit words; tells the receiver where the data payload begins (minimum value = 5, meaning 20 bytes) |
| **Reserved** | 6 bits | Must be zero; reserved for future use |

---

### Control Flags

Six 1-bit flags that control connection state and segment type:

| Flag | Name | Meaning |
|------|------|---------|
| **URG** | Urgent | Urgent Pointer field is valid; segment contains urgent data |
| **ACK** | Acknowledgement | Acknowledgement Number field is valid; set in all segments after the initial SYN |
| **PSH** | Push | Receiver should pass data to the application immediately without buffering |
| **RST** | Reset | Abort the connection immediately; used for error conditions |
| **SYN** | Synchronise | Initiates a connection; carries the sender's ISN |
| **FIN** | Finish | Sender has no more data to send; initiates graceful close |

> [!note] In the three-way handshake: the first segment has only **SYN** set; the server's reply has **SYN + ACK** set; the final ACK has only **ACK** set. See [[TCP Three-Way Handshake]].

---

### Flow Control

| Field | Size | Purpose |
|-------|------|---------|
| **Window** | 16 bits | Receiver's advertised window — how many bytes the receiver is willing to accept beyond the last acknowledged byte; drives flow control |

The window field is how the receiver communicates its buffer capacity back to the sender on every segment. See → [[TCP Window Size and Sliding Window]]

---

### Error Detection

| Field | Size | Purpose |
|-------|------|---------|
| **Checksum** | 16 bits | Covers the TCP header, data, and a pseudo-header derived from the IP header (source/destination IP, protocol, segment length); used to detect corruption in transit |
| **Urgent Pointer** | 16 bits | Offset from the sequence number indicating the end of urgent data; only meaningful when URG flag is set |

---

### Options

| Field | Size | Purpose |
|-------|------|---------|
| **Options** | Variable (0–40 bytes) | Extended functionality negotiated during the handshake |

Common TCP options:

| Option | Purpose |
|--------|---------|
| **MSS** (Maximum Segment Size) | Each side advertises the largest segment it will accept; prevents fragmentation |
| **Window Scale** | Extends the 16-bit window field to allow windows larger than 65,535 bytes (needed for high-speed links) |
| **SACK Permitted** | Negotiates Selective Acknowledgement support |
| **Timestamps** | Enables more accurate RTT measurement and protects against sequence number wraparound on very fast links |

---

## TCP vs UDP Header Comparison

| Feature | TCP | UDP |
|---------|-----|-----|
| Header size | 20–60 bytes (variable) | 8 bytes (fixed) |
| Source/Dest port | ✓ | ✓ |
| Sequence number | ✓ | ✗ |
| Acknowledgement | ✓ | ✗ |
| Flags (SYN/FIN/RST…) | ✓ | ✗ |
| Window size | ✓ | ✗ |
| Checksum | ✓ (mandatory) | ✓ (optional in IPv4) |
| Options | ✓ | ✗ |

The additional fields are precisely what enable TCP's reliability, ordering, and flow control — at the cost of per-segment overhead.

---

## Related Notes

- [[TCP Sequence and Acknowledgement Numbers]] — What the seq/ACK fields contain and how they're used
- [[TCP Window Size and Sliding Window]] — How the Window field drives flow control
- [[TCP Three-Way Handshake]] — Which flags are set at each handshake step
- [[TCP Connection Termination]] — How FIN and RST flags are used
- [[TCP Congestion Control]] — How cwnd interacts with the advertised window
- [[Week 12a]] — Parent note
