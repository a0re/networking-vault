# Lecture 2c

## Ethernet - Collision Domains

- Collisions still happen on a shared network.
- Even with collisions, multiple hosts, the likelihood of collisions increase with the number of hosts.

- **Collision Domain** - Portion of the network where 2 hosts wants to talk at the same time and results in a collision.
- Any network that is shared between two hosts have the ability of colliding.

## Ethernet Bridges

- Full duplex configuration that allows a micro segment between the two computers.

- Micro segments work on full duplex mode to transmit and receive.
- Allowing multiple computers to send frames in their isolated micro segment, virtually eliminating collisions.

## Ethernet Bridges (continued)

- Each port on a switch is an isolated collision domain.
- With only 1 node and communication is in full duplex, there is no chance of collisions.
- Switch creates their own micro-segments by using the MAC address table to forward frames.
- MAC address table stores information which MAC address to each port. The switch populates its table by observing the MAC address of incoming frames and creating an entry for each unique MAC address received port.

- When switches receive a frame: It looks at the destination MAC address, forwards the frame out the port to where the MAC address is connected, and only that port.
- If the frame is a layer 2 broadcast message, then it will send to all ports.
- In hopes of eventually reaching the intended destination.

## Operations

- Ethernet packets contain the source MAC address -Listening to the traffic, it can determine where Ethernet host is in the connected network.

- Bridge connects two shared Ethernet segments together and learns each MAC address.

### Scenario

1. If PC1 wants to send a message to PC3 but doesn't know the MAC.
2. The broadcast ARP will be sent.
3. The switch will receive the broadcast and observe the MAC in source.
4. And store a new entry in MAC address table.
5. The switch then forwards the destination address to all ports.
6. PC3 will eventually receive the frame.
7. PC3 will then reply with an ARP request with its own ARP as a unicast frame with destination MAC as PC1.
8. The switch then receives a unicast frame from port 3 and adds MAC entry table.
9. Destination MAC is observed by the switch and then forwarded to PC1.
10. Exchange frames between PC1 and PC3 without MAC flooding.

[Diagram]

## Collision Domains

- Broadcast packet (`FF:FF:FF:FF:FF:FF`) forwards to all ports with an ARP request (FF means setting all bits to 1).

- Switch will create multiple collision domains -The use of isolation each switch port to its own collision domain / micro segment eliminates collision.

- All switchports in an Ethernet switch will belong in the same broadcast domain, meaning it will send to all ports of the frame.

## Frame Forwarding Methods

Techniques to forward a frame:

**Store & Forward:** The switch will receive the entire frame and verify the CRC (integrity) value before forwarding the frame. If valid, the switch will begin to forward the frame to the destination address and interface / port.

**Cut Through / Fast Forward:** The switch will start forwarding the frame before receiving the entire frame. (Read destination address).

- Lowest level of latency - Minimum destination MAC address of frame before forwarding.
- Fastest but possible to send corrupted frames.

**Fragment-Free Switching:** Wait till the 1st 64 bytes of the frame and then start forwarding the frame.

- Most errors in networking in collision starts on the 1st 64 bytes.
- Compromise between store and forward and cut through, adds a delay but checks the 1st 64 bytes before sending.