# IPv6

## Issues with IPv4

- With population growth, mobile users, transportation and increase of consumer electronics.
- IPv5 was never introduced / released.
- IPv6 is 16 bytes - 128 bits (2^128) IP addresses.
- Dual stacked between both IPv4 and IPv6 support.

## IPv6 Features

- Global reachability and flexibility.
- Aggregation.
- Multi-home.
- Auto configuration.
- End-to-end without NAT.

- Simpler headers:
  - Routing efficiency.
  - No checksums.
  - Performance and forwarding rate.
  - No broadcast addresses.
  - Smaller fields.

## IPv6 Representations

- 4 bits or a nibble is a single hexadecimal digit.

- Total hex values: 32.

- 4 hexadecimals = Hextet.

- 1 character in IPv6 = Nibble.
- 4 characters in IPv6 = Hextet.

## Omitting 0's / Abbreviation

- Leading 0's in any hextet section.
- Omit 0 segments to (::) double colon only once, other 0's would be :0:.

## IPv6 Subnet Mask - Prefix Length

- IPv6 uses slash notation for prefix length.
- Typical prefix length is /64.

- 64 bits - Prefix.
- Interface ID - 64 bits.

## Link-Local Unicast

- Required on every IPv6 interface, generated after global unicast address.
- Link-local address of the router is the default gateway.
- Used by router to identify next hop.