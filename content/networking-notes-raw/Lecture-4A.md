# Handling Problems

The network layer protocol can define methods and mechanisms to deal with communication problems when packets are traveling from source to destination.

### Layer 3 Can Handle Problems

**Network Connectivity:**

- Dealing with packet loss - triggering retransmission but other layers are required.
- Dealing with duplication - addressing duplication, when packet reaches the proper destination.
- ***Warning Trigger:*** To avoid duplication and another host using the same IP.

**Network Routing:**

- Dealing with routing traffic - routing protocols are often in constant communication between neighboring devices.
- If a network path becomes unavailable, routing protocol will recalculate the next optimal path is available.

### Network Throughput Issues

- Mechanisms that would guarantee performance of an application to its minimum requirements.
- Mechanisms that detect bottlenecks and would recalculate.

- IP / Internet Protocol does not have a resource reservation mechanism to guarantee performance.
- Certain IP protocols can avoid bottlenecks and factor each link across the path while processing the best path in the routing table.

### Network Layer Protocols

- IP version 4 and 6 (IPv4) and (IPv6) / Legacy -IPX, AppleTalk, CLNS/DECNET.

### Network Layer Responsibilities

### Best Effort or Guaranteed Throughput

- Best effort protocol doesn't detect packet loss and retransmit lost packet since no mechanism is implemented.
- Nothing will happen to stop this if a packet never reaches its destination. It requires the transport layer to deal with the loss of information.
- Best effort serves the 1st in and 1st out without guarantee or controlling traffic flow (Internet Protocol / IP).
- It is not good enough when latency-sensitive traffic needs to contend with other applications, so quality of service technique to prioritize traffic and guarantee minimum throughput requirements.

### Link Layer Agnostic

- Network layer shouldn't have to change dependent of data layer.

### Network Layer Addressing

- Considerations when considering addressing host:
  1. Unique network-wide addresses.
  2. Design to have a structure - helps routing process / traffic.
  3. Consider the potential network size - why we need IPv6.

- Structure of addressing allows for packet to be routed across multiple networks and standards.
- To work around IPv4, they implemented network address translation techniques to overcome the size of IPv4.

### Routing Decisions

- Each hop on a router will make an independent decision based on routing protocol metrics.
- Routes build their own local routing tables with exit interfaces on known networks.
- Routing table entries do not determine a unique path but considers the best path.

### Network Layer Responsibilities

- The network layer is only responsible for moving data from source host to destination host and upper layers deliver other operations to the right applications.
- Key responsibilities - Provide end-to-end connectivity across multiple link layer networks (Layer 2 and Layer 3):
  - Provide connectivity to devices -not application.
  - Routing connection through network.

### Layer 3 Behaviour

- Layer 3 protocol can be packet or connection based.

1. IP network (packet based) - Internet Protocol.
2. Layer 3 protocol connection based when establishment process, all routers negotiate via control decisions and the best path.

**Connection Based:** Remains the same as long as the connection is alive and the path remains the same between all devices, but adds more overhead in the frame in the communication.

- The pathing is more deterministic and will take the same time to travel to the source address.

**Layer 3:** Enables transfer of data in the interconnected network. The network takes care of addressing, encapsulation, re-encapsulation, and packet routing.

- Network layer takes care of uniquely identifying devices and assigning unique IPs to the network.
- The source host, the application layer, and generates data and the data gets passed layers for transmission over media.
- Layer 3 in this process receives transport layer PDUs and other layer 3 information, e.g., source and destination addresses.

- By encapsulating transport PDUs into a layer 3 header, it forms into packets.
- When data is passed down each layer of the communication, it performs the encapsulation.
- The decapsulation process removes the layer 3 header and deliver the PDUs to the other layers.
- The network layer also routes the packet to their destination based on their routing table.
- Every time a device will observe the destination on the header of the packet and forward the decisions to the next hop.
- Hops is the multiple layer 3 devices a packet transmit from source to destination.
- Routing task by the network layer to ensure the most amount of entries in routing table to make sure the most hop options and exit interface for a known destination network.

[DO SOMETHING HERE]