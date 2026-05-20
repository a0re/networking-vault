# Application and Transport Layers

**Application Layer:** Protocol - DNS/IP addresses.

**Transport Layer:** Receives application layer information for transmission as packets.
- Break down data to smaller segments.

## Benefits of Using a Layered Model

- Standardize models competition from multiple vendors and manufacturers.
- Assist protocol design - Easier for smaller set of specific tasks with defined aspects of communication.
- Layered architecture does not affect other layers.
- Allows protocol and technologies to evolve - Making changes to one layer doesn't mean all layers have to be upgraded - independent of each layer.

# Data Encapsulation

## Protocol Data Units (PDU)

*Each layer has their own form of encapsulation in order to move upstream.*

- **Data** → Session, Presentation, Application.
- **Segment** → {**Transport Header**}{Data} → Transport.
- **Packet** → {**Network Header**}{Transport Header}{Data} → Network.
- **Frame** → {**Frame Header**}{Network Header}{Transport Header}{Data}{Frame Trailer} → Data Link.
- **Bits** → {**Physical**}.

# Physical Layer

- Main function is accept frames coming from data link layer and encode them into bits (0-1) and transmit them over physical media.
- E.g., copper, fibre optic, wireless / radio waves.
- Signals are placed one at a time and receiver will read the signal by decoding into bit stream form into the data link layer into a frame.

# Physical Layer: Purpose

Can be divided between 2 modes:

- **Sending:** Accepts frames from data link layer -transmit bits over media devices.
- **Receiving:** Receive signal over media -pass to data link layer (frame for processing).

Input → PDU Model → Physical / Bits → Encoding Process → Physical / Bits → Data Link / Frame → Output.

- **Encoding:** Converting a stream of bits into predefined code.
- **Signaling:** Physically representing bit streams into media.

## Physical Layer -Fundamentals: Bandwidth

- **Bandwidth** - Capacity to carry data / bits per second -Full capacity.
- **Throughput** - Transmission rate at a period of time.
- **Goodput** - Transmission rate of useable data - without overhead, encapsulation, acknowledgements.

## Manchester Encoding (Optional)

- Encoding method by transmitted digital data.
- Each bit represents signal transition and signify transition.

**Signify in a physical signal:**

- 0 = High → Low.
- 1 = Low → High.

[Note: Insert image later.]

# Lecture 1b / 2a

## Data Link Layer

- Boundary between software communication subsystems and the physical components in communication.
- Allows upper layers in software to access media to physically transmit from one point to another.
- Accepts packets from the network layer (L3 protocol), packages into frame (L2 PDU) and encapsulates them into appropriate media and protocol.

| L3 | L2 |
| --- | --- |
| Layer 3 | Layer 2 |

E.g., wireless interface 802.11 (Ethernet interface). Different types of media and layer 2 protocol.

- **Media Access Control** - If multiple hosts transmit data at the same time, the data frame will collide and corrupt.
- The topology and characteristics of the media will determine media access control rules in L2.

## Nodes - Interconnected Devices (L2 context)

- Purpose: Changing frames between nodes / exchange data between nodes by receiving packets from network layer, encapsulates them into frame, and placing them into the right media from the sender's end.

- **Receiving End:** The frame arrives the media and prepares for passing into upper layers in packet form. It also removes header and trailer information.

## Error Detection Mechanism

- No data has been changed.
- Data sent is correct.
- Data hasn't been corrupted.

- If data is corrupt, it will be dropped instead of passing to upper layer. The "operator" will deal with the loss of data but will not waste time processing corrupted information.

- The receiving end will evaluate the numerical value using the data.
- The receiving end will do the same operation.

# Data Link Layer -Sublayers

## Logical Link Control (LLC)

- Layer 2 upper layer subsystem.
- Implemented in software in charge of interacting with upper layers and network layers.
- Add header information that identifies layer 2 protocol to build the packet to be sent.
- Allow different layer 3 protocols to use the same media and network interface cards.
- Allows different layer 3 protocols at the same time.
- LLC ensures packets IPv4 and IPv6 packets transmitted from the same connections.

## Media Access Control (MAC) Sublayers

- Functions - Performed in hardware and provide L2 addressing and encapsulation to the media type.
- Lower layer subsystem.
- Addressing the identification of multiple nodes.
- MAC sublayer provides Ethernet framing functions to encapsulate the packet and produce Ethernet frames but also encapsulate into a Bluetooth frame.
- The use of subdividing the data link layer is to decouple upper layer protocol and the type of media and decoupling functions implemented in software.

## Media Access Control (MAC) 

- Every hop onto different types of devices / nodes, the frame is "broken/decapsulated" and forwarded into a new frame.
- Headers of frames are formatted to the specific medium / layer.
- The data will remain the same (Layer 2).

Decoupling Layer 2 from Layer 3 protocol, the same packet can transmit across different interconnection types without changes in the packet at each hop of the communication.

- The more layer 2 protocols requires with different frame formats (headers, trailers) require more control information in the layer 2 headers.

[Insert Diagram Example]

# Data Link Layer -Formatting Data for Transmission

## Layer 2 Generic Frame Format

`{Header}{Packet (Data)}{Trailer}`

[Diagram]

- **Header:** Frame start, addressing, type and quality control.
- **Trailer:** Error detection, frame encapsulation.

- Point-to-point protocol:
  - Doesn't need addressing information.
  - Ethernet requires addressing information and VLAN ID.

**Frame Start:** Indicate start and end (Layer 2).

**Addressing:** Source and destination addressing identifiers of nodes in the communication with MAC address. The addressing identifies the sender and receiver.

**Type:** Include type of payload / packet (Layer 3 protocol) - IPv4/6.

**Quality Control:** (QoS) Quality of Service - signify priority levels to the frame, voice packets.

**Error Detection:** Numerical value calculation.

**Controlling Access to the Media:** Media access control depends on network topology and nodes method to share media.

**Network Topologies**

- **Physical:** Nodes in the network are physically connected with each other by transmission media.
- **Logical:** How nodes are arranged according to data link layer, visual path of the frames are transmitted. Can be done without physical cable.