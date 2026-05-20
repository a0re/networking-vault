---
tags: [networking, ipv4, addressing, subnetting, week-4]
week: 4
aliases: [IPv4, IP Address, Subnet Mask, CIDR, Fragmentation, NAT, Private IP]
---

# IPv4 Addressing

> **Source:** [[Week 4]]

## Address Structure

IPv4 addresses are **32-bit values**, written as four 8-bit octets in dotted decimal notation:

```
192 . 168 . 10 . 11
 ↑       ↑       ↑    ↑
 Octet 1  2       3    4     (each 0–255)
```

### Network vs Host Portion

A **subnet mask** (also written as CIDR prefix length) divides the address into two parts:

```
IP:   192.168.10.11 /24
      └──────────┘ └─┘
      Network       Host
      192.168.10    .11

Subnet mask: 255.255.255.0
```

The `/24` means the first 24 bits are the network portion. The remaining 8 bits identify the host.

### Reserved Addresses in a Subnet

| Address | Role |
|---------|------|
| First address (e.g. `.0`) | **Network address** - identifies the subnet, not usable by hosts |
| Last address (e.g. `.255`) | **Broadcast address** - reaches all hosts in the subnet |
| Typically `.1` or last usable | **Default gateway** - convention only |

---

## Legacy Classful Addressing

Before CIDR, the first octet determined the class:

| Class | First Octet | Subnet Mask | Hosts Per Network | Use |
|-------|-------------|-------------|-------------------|-----|
| A | 1–127 | /8 | ~16.7 million | Large networks |
| B | 128–191 | /16 | ~65,000 | Medium networks |
| C | 192–223 | /24 | 254 | Small networks |
| D | 224–239 | - | - | Multicast |
| E | 240–255 | - | - | Experimental |

> [!note] Classful addressing is largely obsolete - CIDR and subnetting replaced it. But the class A/B/C ranges are still useful to recognise.

---

## Public vs Private Addresses

Not all addresses are routable on the Internet. Private ranges are for internal use only - routers are configured to **never forward private IPs externally**.

### Private Ranges (RFC 1918)

| Range | CIDR | Use |
|-------|------|-----|
| `10.0.0.0 – 10.255.255.255` | /8 | Large internal networks |
| `172.16.0.0 – 172.31.255.255` | /12 | Medium internal networks |
| `192.168.0.0 – 192.168.255.255` | /16 | Home/small office networks |

**NAT (Network Address Translation)** is used to allow private-addressed hosts to communicate with the Internet by translating their address at the network boundary.

---

## Special Use Addresses

| Range | Purpose |
|-------|---------|
| `127.0.0.1 – 127.255.255.255` | **Loopback** - traffic directed to self |
| `169.254.0.0 – 169.254.255.255` | **Link-local** - auto-assigned when no DHCP (APIPA) |
| `192.0.2.0 – 192.0.2.255` | **TEST-NET** - documentation and examples only |
| `240.0.0.0 – 255.255.255.254` | **Experimental** - reserved |
| `255.255.255.255` | **Limited broadcast** - all hosts on local network |

---

## IPv4 Packet Header Fields

Minimum header size: **20 bytes** (without optional fields).

| Field | Size | Purpose |
|-------|------|---------|
| **Version** | 4 bits | IP version (4) |
| **IHL** (Internet Header Length) | 4 bits | Length of the header |
| **Differentiated Services (DS)** | 8 bits | QoS marking - traffic type and priority |
| **Total Length** | 16 bits | Total packet size (header + data) |
| **Identification** | 16 bits | Identifies fragments belonging to the same original packet |
| **Flags** | 3 bits | Fragmentation control (DF, MF bits) |
| **Fragment Offset** | 13 bits | Position of this fragment in the original packet |
| **Time-To-Live (TTL)** | 8 bits | Decremented at each hop; packet dropped at 0 - prevents infinite loops |
| **Protocol** | 8 bits | Upper layer protocol (6=TCP, 17=UDP) |
| **Header Checksum** | 16 bits | Integrity check for the header |
| **Source IP** | 32 bits | Sender's IP address |
| **Destination IP** | 32 bits | Receiver's IP address |

> [!tip] **TTL** is the IP equivalent of a loop-prevention mechanism. IPv6 calls this the Hop Limit.

---

## IP Fragmentation

When a packet is larger than the **MTU (Maximum Transmission Unit)** of a link, it must be fragmented. Ethernet's MTU is typically **1500 bytes**.

| Flag Bit | Value | Meaning |
|----------|-------|---------|
| **DF** (Don't Fragment) | 1 | Do not fragment this packet; drop and send ICMP error if too large |
| **MF** (More Fragments) | 1 | More fragments follow |
| **MF** | 0 | This is the last (or only) fragment |

The **Fragment Offset** field tells the receiver where this fragment fits in the reassembled packet. The first fragment has an offset of 0. Offset is in units of **8 bytes**.

### Fragmentation Example

```
Original packet: 4000 bytes data, MTU = 1500 bytes
Header = 20 bytes → usable data per fragment = 1480 bytes

Fragment 1: bytes 0–1479,    offset = 0,   MF=1
Fragment 2: bytes 1480–2959, offset = 185, MF=1
Fragment 3: bytes 2960–3999, offset = 370, MF=0
```

---

## Transmission Types

| Type | Sender | Receivers | Example |
|------|--------|-----------|---------|
| **Unicast** | 1 host | 1 specific host | Normal web traffic |
| **Broadcast (Limited)** | 1 host | All hosts on local L2 network | `255.255.255.255` |
| **Broadcast (Directed)** | 1 host | All hosts in a specific subnet | `192.168.1.255` |
| **Multicast** | 1 host | Selected group of subscribers | `224.0.0.0–239.255.255.255` |

---

## IPv4 Limitations

- Total address space: ~4.3 billion (`2^32`)
- Not enough for every device - exhausted around 2011
- Workarounds: NAT, CIDR, private addressing
- Long-term solution: **IPv6** (128-bit addresses, `2^128` space)

---

## Related Notes

- [[Network Layer (IP)]] - Layer 3 concepts and responsibilities
- [[ARP (Address Resolution Protocol)]] - resolves IPv4 addresses to MAC
- [[IPv4 Routing]] - how routers forward packets between networks
- [[Ethernet & MAC Addressing]] - Layer 2 that carries IPv4 packets
