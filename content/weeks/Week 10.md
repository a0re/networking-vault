---
tags: [networking, week-10, ipv6]
week: 10
---

# Week 10 - IPv6

## Topics Introduced This Week

- [[IPv6 Addressing]] - Address structure, representation, abbreviation rules, address types, global unicast hierarchy

## Key Concepts

**IPv4 exhaustion** drove the need for IPv6. With ~4.3 billion IPv4 addresses and every device needing one, the address space ran out around 2011. IPv6 provides `2^128` addresses - effectively unlimited.

**IPv6 addresses are 128 bits**, written as 8 groups of 4 hex digits (hextets). Leading zeros and consecutive zero groups can be abbreviated, making addresses more readable.

**No NAT required** - every device can have a globally routable address, restoring true end-to-end connectivity that NAT breaks in IPv4.

**Link-local addresses** (`FE80::/10`) are auto-generated on every IPv6 interface and serve as the default gateway address - routers advertise themselves using their link-local address.

## Connections to Other Weeks

- IPv6 contrasts with [[IPv4 Addressing]] (Week 4) - same logical concepts, very different structure
- The routing principles from [[IPv4 Routing]] (Week 6) apply equally to IPv6
