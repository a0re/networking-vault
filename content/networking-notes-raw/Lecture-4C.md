# ARP

- Hosts that have the same network portion will know that they are in the same network.
- ARP is a Layer 2 broadcast process message received by all hosts in the same Layer 2 network.
- Encapsulated in the Ethernet frame with IP target information (no IP headers).

## Internal Network

- Unicast ARP request has the destination MAC and source MAC.
- After sending an ARP request back, it is added to the ARP cache table.

[ARP Example]

- ARP cache entries will be stored by both the initiator and the replier for each other's addresses.
- ARP cache is stored in memory so it can timeout or be removed for new entries.

## Extended Network

- If a host needs to communicate to another outside its network, the ARP packet will request the MAC address of the router since it is the Layer 2 boundary.
- Destination MAC will be the router.

TL;DR?

## MAC Address

- Address does not change.
- Physical address - assigned by host NIC.
- Data link layer address (Layer 2).
- 48 bits.

## IP Address

- Layer 3 address (network address).
- 32 bits, dotted decimal (expand on this).
- Assigned by host or network admin.
- Also known as logical address.

- Both are required because of the layered architecture.

- A router examines the IP addresses.
- A switch examines the MAC addresses.

- Within local network, a packet will forward devices with Layer 2 addresses (MAC).
- When communicating across different networks with routers, it will use Layer 3 (network address).

- When traveling to other networks, routers would modify the MAC addresses in the packet -since MAC addresses are locally significant in Layer 2.

## ARP (Addressing Resolution Process)

- Used to find MAC address with only IP address.
- Layer 3 knows the hostname and destination IP address and can encapsulate that message in the header (Layer 3).
- Layer 2 subsystem will encapsulate the source and destination address.