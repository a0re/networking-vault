# IPv6 Address Types

**Reserved address:**

- Unspecified: `::/128`
- Loopback: `::1/128`
- Multicast: `FF00::/8` --> `FFFF`
- Link local unicast: `FE80::/10` --> `FEBF`
- Global unicast: `2XXX::/4` or `3XXX::/4`
- Reserved (future global unicast): Everything else.

## Unicast Address

- Uniquely identify an IPv6 device.
- Packets that are sent to a unicast address.
- Receives the interface assigned to that address.

## Types of Unicast

- Global unicast - globally unique, routable, static or assigned dynamic.
- Link local unicast.
- Loopback.
- Unspecified address.
- Unique local.
- Embedded IPv4.

## Global Unicast Hierarchy

- IANA: /16
- Registry: /23
- ISP prefix: /32
- Site prefix: /48
- Subnet prefix: /64

- Subnet ID - 16 bits can be allocated from 5 to 65,536 subnets.
- Can borrow interface ID to create additional subnets.