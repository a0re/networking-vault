---
tags:
  - networking
  - transport-layer
  - week-11
week: 11
aliases:
  - Week 11a
  - Transport Layer
  - Port Numbers
---

# Week 11a — Transport Layer & Port Numbers

> **Source:** Lecture 11a — TNE10006/TNE60006 Networks and Switching

## Overview

The transport layer (Layer 4) sits above IP (Layer 3) and enables communication between **applications** on different devices. While IP gets packets from host to host, the transport layer delivers data to the correct **application** on each host.

| Layer | Role |
|-------|------|
| Network (IP) | Host-to-host delivery |
| Transport | Application-to-application delivery |

---

## Responsibilities of the Transport Layer

### Application Identification

- Applications use **names** (not IP addresses) — DNS handles name-to-IP translation
- The transport layer connects source and destination applications, abstracting away host type, media, routing, and network congestion
- Each conversation between a source and destination application is tracked independently

### Segmentation & Reassembly

- Application data streams are **segmented** into appropriate-sized units for network transmission
- Each segment is carried in the payload of a Layer 3 packet
- The receiving transport layer **reassembles** the data stream before passing it to the application

### Multiplexing

- Multiple applications can communicate concurrently over the same network interface
- Each conversation is uniquely identified by a **tuple**: (Source IP, Source Port, Destination IP, Destination Port)

---

## The Two Transport Protocols

There are two distinct transport layer protocols, chosen based on application requirements:

| Protocol | Type | Best For |
|----------|------|----------|
| **[[Week 11b\|UDP]]** | Connectionless, best-effort | Real-time transmission, low latency |
| **[[Week 12a\|TCP]]** | Connection-oriented, reliable | Correctness, ordered delivery |

- **Real-time applications** (VoIP, video streaming) prioritise speed over reliability — they benefit from UDP's low overhead
- **Reliable applications** (web, file transfer, email) need acknowledgement, retransmission, and ordering — they use TCP

The choice of protocol significantly impacts application performance under network conditions.

---

## Port Numbers

Both TCP and UDP use **port numbers** to multiplex multiple services:

- 16-bit values (0–65535)
- Each segment carries a **source port** and **destination port** in its header
- Server-side applications use **well-known** or **registered** ports
- Client applications use **ephemeral** (randomly assigned) ports

| Type | Range | Examples |
|------|-------|----------|
| Well-known | 0–1023 | HTTP (80), HTTPS (443), DNS (53) |
| Registered | 1024–49151 | |
| Ephemeral | 49152–65535 | Client-side random ports |

> [!note] Many ports are registered for both TCP and UDP (e.g. DNS uses port 53 for both, but typically runs over UDP).

---

## Related Notes

- [[Week 11b]] — UDP protocol details
- [[Week 12a]] — TCP fundamentals
- [[TCP vs UDP]] — Comparison of the two transport protocols
- [[TCP Header Format]] — TCP header fields
- [[Lecture-02b]] — Ethernet, CSMA/CD, MAC addressing (Layer 2 foundations)
