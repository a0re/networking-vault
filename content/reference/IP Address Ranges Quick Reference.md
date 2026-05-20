---
tags: [networking, reference, ipv4, addressing]
---

# IP Address Ranges Quick Reference

## Private Ranges (RFC 1918 -not routable on Internet)

| Range | CIDR | Class | Hosts |
|-------|------|-------|-------|
| `10.0.0.0 - 10.255.255.255` | /8 | A | ~16.7 million |
| `172.16.0.0 - 172.31.255.255` | /12 | B | ~1 million |
| `192.168.0.0 - 192.168.255.255` | /16 | C | ~65,000 |

---

## Special Use Addresses

| Range | Purpose |
|-------|---------|
| `127.0.0.0/8` | Loopback (localhost) |
| `169.254.0.0/16` | Link-local / APIPA (no DHCP) |
| `192.0.2.0/24` | TEST-NET (documentation only) |
| `224.0.0.0/4` | Multicast |
| `255.255.255.255` | Limited broadcast |

---

## Multicast Ranges

| Range | Scope |
|-------|-------|
| `224.0.0.0 - 224.0.0.255` | Link-local (not forwarded by routers) |
| `224.0.1.0 - 238.255.255.255` | Global scope |
| `239.0.0.0 - 239.255.255.255` | Administratively scoped |
| `224.0.1.1` | NTP (Network Time Protocol) |

---

## Classful Ranges (Legacy)

| Class | First Octet | Default Mask | Hosts/Network |
| ----- | ----------- | ------------ | ------------- |
| A     | 1-127       | /8           | ~16.7 million |
| B     | 128-191     | /16          | ~65,534       |
| C     | 192-223     | /24          | 254           |
| D     | 224-239     | -            | Multicast     |
| E     | 240-255     | -            | Experimental  |

---

## VLAN ID Ranges

| Range | Category | Storage (Cisco) |
|-------|----------|----------------|
| 0 | Reserved | -|
| 1 | Default VLAN | Flash |
| 2-1005 | Normal range | Flash |
| 1006-4094 | Extended range | Running config |
| 4095 | Reserved | -|

---

## Subnet Quick Reference

| CIDR | Subnet Mask | Hosts |
|------|-------------|-------|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |
| /27 | 255.255.255.224 | 30 |
| /28 | 255.255.255.240 | 14 |
| /29 | 255.255.255.248 | 6 |
| /30 | 255.255.255.252 | 2 *(point-to-point)* |

---

## IPv6 Address Types

| Address / Range | Type | Notes |
|----------------|------|-------|
| `::/128` | Unspecified | IPv4 equivalent: `0.0.0.0` |
| `::1/128` | Loopback | IPv4 equivalent: `127.0.0.1` |
| `FE80::/10` | Link-Local Unicast | Auto-generated, not routed, used as default gateway |
| `FC00::/7` | Unique Local | IPv4 equivalent: RFC 1918 private ranges |
| `FF00::/8` | Multicast | All addresses starting with `FF` |
| `2000::/3` | Global Unicast | Publicly routable (2xxx or 3xxx prefix) |

### Global Unicast Hierarchy

| Prefix | Allocated To |
|--------|-------------|
| /16 | IANA |
| /23 | Regional Registry (ARIN, APNIC, RIPE, etc.) |
| /32 | ISP |
| /48 | Organisation / Site |
| /64 | Subnet (16-bit subnet ID, 65,536 possible subnets) |

---

## Related Notes

- [[IPv4 Addressing]] -full explanation of IPv4 addressing concepts
- [[IPv6 Addressing]] -full explanation of IPv6 addressing concepts
- [[ARP (Address Resolution Protocol)]] -how IPs resolve to MACs
- [[Protocol & Standard Quick Reference]]
