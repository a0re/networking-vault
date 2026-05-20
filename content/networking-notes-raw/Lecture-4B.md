# Legacy Classful Addressing

- IETF defined all IPv4 ranges.

**Classes:**

- **Class A:** Subnet mask (18), 2^24 - 2 addresses, 1 - 127 (1st octet range).
- **Class B:** Subnet mask (16), 2^16 - 2 addresses, 128 - 191.
- **Class C:** Subnet mask (24), 2^8 - 2 addresses, 192 - 223.
- **Class D:** 224 - 239 (NA Multicast).
- **Class E:** 240 - 255 (NA Experimental).

**Regional Internet Registries (RIR):**

Companies that manage and assign IPs around the world.

## Limitations of IPv4

- Total IPv4 address - 4,294,967,296 unique addresses.
- Each router has multiple IP addresses.
- Every device needs an IP address.
- IPv4 addresses are $25 per IP.
- NAT - complicated.
- End-to-end connectivity - but complicated.

## Packet Header / IP Addressing

Translating hostnames / URLs is done by DNS -Domain Name System.

IP addresses are 32 bits.

**Hierarchical system / addressing.**

Format - Broken to 4 octets.

- Each byte is dotted decimal (255).

`00000000 = 8`

Positions of 1's is 2^(bits). 0-7.

**Network portion / Host portion** - Devices use a separate 32-bit pattern called subnet mask.

`IP: 192.168.10.11/24`

- 192.168.10 - Network portion.
- 11 - Host portion.

**Subnet mask:**

`255.255.255.0`

- Length of the network portion is used to find the host portion.

**Network + Broadcast** - Unusable addresses.

- Reserved for networking purposes.
  - E.g., 192.168.5.0 - 192.168.5.255.
- Designed to reach every host in the network, 1st usable IP.
  - E.g., .1 - .254.

`/24` e.g., 10.1.1.1/24.

1st or last IP address as default gateway because of convention.

## Public and Private IPv4 Addresses

- Not all IP addresses are routable through the internet.
- Private IP networks - used by organizations but not outside and on the internet.

- If host needs to communicate beyond the organisation, a packet can't go beyond the boundaries of the IP address, routers were configured to never forward private IPs.

- NAT (Network Address Translation) to work around that.

- Hosts that do not require access to internet use private addresses:
  - 10.0.0.0 → 10.255.255.255 (10.0.0.0/8).
  - 172.16.0.0 → 172.31.255.255 (172.16.0.0/12).
  - 192.168.0.0 → 192.168.255.255 (192.168.0.0/16).

**Special Use IPv4:**

- Network and broadcast - 1st and last address.
- Loopback address - 127.0.0.1 & 127.255.255.255 - Direct traffic to themselves.
- Link-local address - 169.254.0.0 & 169.254.255.255 - Automatically assigned to localhost.
- TEST-NET address - 192.0.2.0 - 192.0.2.255 -Teaching / learning purposes / documentation.
- Experimental address - 240.0.0.0 & 255.255.255.254 - Listed as reserved.

## Connectionless

- Doesn't have dedicated end-to-end connection between the source and destination.
- Doesn't keep track of the communication state.
- Doesn't validate the receiver has connected before sending.
- Doesn't acknowledge that packets have arrived or process the packet.
- Receiver doesn't know packets are coming.

- An analogy is sending a letter:
  - Connectionless means no initial establishment.
  - Added header controls to keep track of the session.
  - No session establishment, acknowledgement, termination, etc.
  - If packets are lost, the IP layer of the sender will not be made aware.
  - IP is easy to send and receive information which is advantageous in terms of speed.

**Best Effort Delivery:** 1st in, 1st out process with prioritization of flow control techniques.

- Keep track if delivered or corrupted - unreliable. Upper layers take care of it.
- IP is packet based and decisions are made independently different paths.
- Transport layer can keep track of information delivery, reception and acknowledge retransmission.
- Transport layer mitigate the issues of IP.

## IPv4 Packet Header

- Minimum length of 20 bytes + optional field.
- If padding / optional field used.
- **Version field** - 4 bits long or 1st byte.

- **Differentiated Services (DS)** - 8 bits - Mark with type of service or priority in the network on the packet.
- Packets are marked with a code identifying type of traffic carrying and priority level.

### Time-To-Live

- How long to carry for before being dropped.
- A number is decreased every time in a layer 3 hop.
- Packet is dropped when reaching 0.
- Dropping looping packets with loop forever.
- 8-bit long field (255 hops).

### Protocol Field

Indicate upper layer protocol.

- TCP / IP indicate transport is TCP or UDP.

### Source IP

Sender of the packet.

### Destination IP

Receiver.

### Internet Header Length

- 4 bits / 1st byte.
- Indicates the length the IP header length.

### Total Length

Indicate total length of packet / payload.

### Header Checksum

- Validate number of the payload has not been corrupted while traveling across the network.
- Receiver performs a calculation of the data to check.

## Media Independence

- Designed to be media independent.
- IP doesn't require lower layer protocol / media.
- Same over copper and fibre and copper serial.

- Output of IP layer will be processed by the data link layer to be placed onto the media.

- Data link layer is in charge of taking the IP packet and encapsulating it to the appropriate physical layer.

- **Maximum Transfer Unit (MTU)** - Important when transferring data.
  - Maximum PDU size it can carry across the link.
  - E.g., Ethernet frames can carry 1500 bytes on the link.

- Data link layer informs the network layer the MTU value.
  - To not exceed this length.
  - Packets need to be broken to appropriate size.

## IP Fragmentation / Packet Header

- Split packets into smaller segments to send over a network.
- Allowing complete media independency and free to transmit between the physical media and data link layer.

### Identification

- 16-bit value used to identify all the values belonging to the same packet.
- Randomly chosen 2^16 values unlikely to be the same ID when transmitting.

### Flags

- 3 bits long - indicate if a packet is a fragment of a larger fragment - last fragment of a newer fragmented.

- **DF-Bit:** Set to 1 - Don't fragment.
- **MF-Bit:** Set to 1 - Continue fragmenting.
- Set to 0 - No fragment at all / last fragment.

### IP Fragmentation Offset

- Field specifies the offset, position in the message.
- Indicates the position of the 2nd byte of the original packet.
- The identification header allows the receiving end to gather all the fragmented packets but doesn't know the order.

- 1st fragment has an offset of 0.
- The unit is specified in units of 8 bytes (64 bits).

[Example]

- 19980 bytes data / 12000 bytes (header).
- 3300 bytes MTU.
- 3300 - 20 = 3280 (multiplied by 8?).

- F1. Bytes: 0-3278.
- F2. Bytes: 3280-6559.
- F3. Bytes: ...

- Square 8 3280 = 410?

Number of frames = Original packet size / (MTU - Header Size).

## Unicast Transmission

In IPv4 networks can communicate 3 different ways.

### Unicast

- Sending a packet from 1 host to another.
- Contain source IP and destination IP.

### Broadcast

- 2 types of broadcast - limited and directed.
- **Limited:** Packet send to all layer 2 network. Uses the universal broadcast IP (255.255.255.255) as the destination IP. Limited to layer 2 and does not forward the broadcast packet.

**Directed:** Reach all hosts but from outside from the host network. Never originated from the destination network. By default, routers do not forward broadcast but can be configured to do so.

Both limited and directed use the universal MAC address as the destination MAC.

### Multicast

- Process of sending a packet from 1 host to a selected group of hosts.

- Reserved addressing: 224.0.0.0 → 239.255.255.255.
- Link local: 224.0.0.0 → 224.0.0.255.
- Global scope address: 224.0.1.0 → 228.255.255.255.
- 224.0.1.1 Reserved for network time protocol.