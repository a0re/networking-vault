---
tags: [networking, ipv6, addressing, layer-3, week-10]
week: 10
aliases: [IPv6, IPv6 Address, Global Unicast, Link-Local, Hextet, Prefix Length]
---

# IPv6 Addressing

> **Source:** [[Week 10]]

## Why IPv6?

**IPv4 exhaustion** - ~4.3 billion addresses (`2^32`) were never enough for a world of billions of people, each with multiple devices. IANA exhausted its IPv4 pool in 2011.

Drivers of demand:
- Population growth
- Mobile devices (every phone needs an IP)
- IoT (appliances, vehicles, sensors)
- Consumer electronics

> [!note] IPv5 was an experimental streaming protocol that was never publicly released - the version number 5 was skipped to avoid confusion.

**IPv6** provides `2^128` addresses - approximately 340 undecillion. Every device on Earth can have a globally unique, routable address.

---

## IPv6 vs IPv4 Key Differences

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Address length | 32 bits | **128 bits** |
| Address format | Dotted decimal | Colon-separated hex |
| NAT required | Yes (address shortage) | **No** - true end-to-end |
| Header checksum | Yes | **No** (removed for performance) |
| Broadcast | Yes | **No** - replaced by multicast |
| Auto-configuration | DHCP only | Built-in (SLAAC) |
| Header size | Variable (20+ bytes) | Fixed (40 bytes) |

---

## Address Format

IPv6 addresses are **128 bits**, written as **8 groups of 4 hexadecimal digits** separated by colons:

```
2001:0DB8:0000:0000:0000:FF00:0042:8329
```

### Terminology

| Term | Meaning |
|------|---------|
| **Nibble** | 1 hex digit = 4 bits |
| **Hextet** | 4 hex digits = 16 bits (one group) |

An IPv6 address has **8 hextets** = 32 hex digits = 128 bits.

---

## Abbreviation Rules

IPv6 addresses can be shortened using two rules:

### Rule 1 - Omit Leading Zeros Within a Hextet

```
2001:0DB8:0000:0000:0000:FF00:0042:8329
→   2001: DB8:   0:   0:   0:FF00:  42:8329
```

### Rule 2 - Replace One Consecutive Run of All-Zero Hextets with `::`

```
2001:DB8:0:0:0:FF00:42:8329
→   2001:DB8::FF00:42:8329
```

> [!warning] `::` can only be used **once** in an address. If used twice, it's ambiguous - the router can't determine how many zero groups each `::` represents.

### Examples

| Full Address | Abbreviated |
|-------------|-------------|
| `FE80:0000:0000:0000:0202:B3FF:FE1E:8329` | `FE80::202:B3FF:FE1E:8329` |
| `2001:0DB8:0000:0001:0000:0000:0000:0001` | `2001:DB8:0:1::1` |
| `0000:0000:0000:0000:0000:0000:0000:0001` | `::1` (loopback) |
| `0000:0000:0000:0000:0000:0000:0000:0000` | `::` (unspecified) |

---

## Prefix Length

IPv6 uses **slash notation** (CIDR) just like IPv4 - no subnet masks.

```
2001:DB8:ACAD:1::1/64
                   ↑
                   64-bit network prefix, 64-bit interface ID
```

The typical prefix length is **/64**:
- First 64 bits = **network prefix**
- Last 64 bits = **interface ID** (often derived from the MAC address via EUI-64)

---

## IPv6 Address Types

### Reserved / Special Addresses

| Address | Purpose |
|---------|---------|
| `::/128` | **Unspecified** - equivalent to IPv4 `0.0.0.0` (source before address assignment) |
| `::1/128` | **Loopback** - equivalent to IPv4 `127.0.0.1` |
| `FF00::/8` | **Multicast** - all addresses starting with `FF` |
| `FE80::/10` | **Link-Local Unicast** - auto-generated, not routed |

### Unicast Address Types

| Type | Range | Routable? | Notes |
|------|-------|-----------|-------|
| **Global Unicast** | `2000::/3` (2xxx or 3xxx) | Yes - globally | Equivalent to public IPv4 |
| **Link-Local** | `FE80::/10` | No - link only | Auto-generated, used for gateway |
| **Loopback** | `::1/128` | No | Internal testing |
| **Unspecified** | `::/128` | No | Placeholder before address assigned |
| **Unique Local** | `FC00::/7` | No - site only | Equivalent to private IPv4 (RFC 1918) |
| **Embedded IPv4** | - | - | Transition mechanism |

---

## Link-Local Address

- **Required on every IPv6 interface** - automatically generated even without manual configuration
- Range: `FE80::` to `FEBF::`
- **Never forwarded by routers** - only meaningful on the local link
- The **router's link-local address is the default gateway** for hosts on that segment
- Routers use link-local addresses to identify next-hop routers in routing protocols

```
Host default gateway: FE80::1   ← router's link-local address on this segment
```

---

## Global Unicast Address Hierarchy

Global unicast addresses follow a hierarchical allocation:

```
IANA                → /16  (top-level allocation)
Regional Registry   → /23  (RIR: ARIN, APNIC, RIPE, etc.)
ISP                 → /32  (ISP prefix)
Organisation/Site   → /48  (site prefix)
Subnet              → /64  (subnet prefix)
Interface ID        → last 64 bits
```

### Subnet ID

The 16 bits between the /48 site prefix and the /64 interface ID give an organisation:
- **2^16 = 65,536 possible subnets** - no need for subnet mask calculations like IPv4

---

## Dual Stack

During the IPv4→IPv6 transition, devices run **dual stack** - both IPv4 and IPv6 simultaneously:
- A device has both an IPv4 and IPv6 address
- Prefers IPv6 when available, falls back to IPv4
- Allows gradual migration without a hard cutover

---

## Related Notes

- [[IPv4 Addressing]] - contrast with IPv4 structure and limitations
- [[Network Layer (IP)]] - Layer 3 context
- [[IPv4 Routing]] - routing principles apply equally to IPv6
- [[IP Address Ranges Quick Reference]] - updated with IPv6 ranges
