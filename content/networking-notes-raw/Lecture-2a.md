# Lecture 2a

# Controlled Access

- Only 1 device / node can send data at a time with a token.

- Only node / controller node will pass token so the network node allows data to send to media. If data isn't required to be sent by node and has the token, the node will pass the token without sending anything.

- Legacy technologies even though access controls will result in no collisions, the token passing method delays collision because it cycles through each node with the token if data needs to be sent. Very inefficient.

## Fibre Data Distributed Interface

- Topology of network determines the access control mechanism used, that also impacts the frame format with more header and trailer information.

# Frame

## Frame Environment

- Satellite communication and environmental impact can affect the quality of the information. More controls are needed to deliver and adding more header and trailer information into the frame but requires more data to transmit.

## Protected Environment

- A frame is guaranteed with its destination, it requires less control, smaller fields, smaller frames helping transmission rate.

**A balance is required between control, frame format and transmission rate to avoid too much overhead.**

## Duplex

- The devices / nodes have to agree on the settings because they have to know each other, otherwise collision occurs.

## Contention-Based Access

*Topology is a shared media.*

- **Multi-access Network** - Multiple nodes sharing media requiring more complexity than point to point, over media being used and requires more control.

- **Contention-based access:** All nodes compete to use media. Devices don't take turns to access and send traffic whenever ready and collisions occur on media.

## Carrier Sense Multiple Access w/ Collision Detection (CSMA/CD)

- Network interface cards: Check if data is being sent until media is idle, node will send traffic while sending to detect if data / frame collided.

- When data collides on media, a jamming signal is created on the media. A node will detect a collision, the backoff timer is started and then set again.

- For Ethernet networks 802.3.

## Carrier Sense Multiple Access w/ Collision Avoidance (CSMA/CA)

- Nodes listen if media has data frame, then pause and back off but if media is clear they will send a notification to send data. This notification will be received by a central node that controls communication. The node will be given a clear to the sender.

- Controller nodes ensure no 2 nodes send data at the same time.
- Collisions are still possible with this method.

# Topology

**Logical:** Virtual arrangement of nodes independent of their physical connectivity. Data link layer sees logical topology. Influences network frame and MAC address.

Media access control depends on logical topology and the distribution of the nodes.

# Common Physical WAN Topology (Wide Area Network)

[Insert point-to-point] [Insert hub and spoke] [Insert full mesh]

- **Point-to-Point:** 1 direct physical connection.
- **Hub and Spoke:** Multiple nodes, 1 hub to every point-to-point.
- **Full Mesh:** Point-to-point all interconnected.

## Point-to-Point Characteristics

- Nodes do not need to share media or transmit media with other nodes.
- **Physical:** Point-to-point doesn't require addressing.
- **Logical:** Logical point can be established in the data link layer protocol to simulation.
- If 1 path, collision isn't possible since no other nodes exist in 1 lane.

# Half & Full Duplex

## Half Duplex

- 1 lane to send and receive, actions need to be completed first before another.

[Insert Image]

## Full Duplex

- 2 lanes in communication to send and receive. Server allows 2 nodes to send and receive at the same time.

[Insert Image]

# Layer 2 Address

- Topology affects the layer 2 addressing (MAC) and the addressing fields when inserting to the header.
- Devices / Nodes in shared media will be required to identify themselves when sending frames/data and the intended receiver in communication.

**Add MAC address** (Multi-Access Topology).
Layer 2 addressing fields into the frame while point-to-point addressing information no need for addressing information.