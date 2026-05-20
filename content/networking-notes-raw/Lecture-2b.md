# Lecture 2a (continued)

## Lecture 2 Address

- Topology affects the layer 2 addressing (MAC) and the addressing fields when inserting to the header.
- Devices / Nodes in shared media will be required to identify themselves when sending frames/data and the intended receiver in communication (adding MAC address).
- Layer 2 addressing fields into the frame while point-to-point addressing information no need for addressing information.

## Ethernet

- Original concept of Ethernet protocol is based off of ALOHA network - First wireless packet data network / radio-based network (media transmission).
- Radio waves are the shared medium - anything in the covered area used a transceiver tuned to the right frequency - might get access to the media.
- Radio communication is a shared access communication and describes a complex and effective media access control mechanism.
- 1st communication standard release was 1933 and adopted media access control mechanism of ALOHA network.
- The predominant use is due to the necessity for Ethernet technologies with LAN networks with shared media access.
- **Modern Ethernet Switches:** No longer require media access control and forwards data and collisions are actually possible because of media access control redundancy.

## Ethernet History

- Ethernet protocol still implements / defines media access control mechanism, allows for backward compatibility.

## Ethernet Operations

- Ethernet protocol is a family of standards which collectively describes the data link layer and physical layer communication function.

[Diagram of Data Link & Physical]

- Ethernet sits in the layer 1 model and layer 2 OSI model.
- Defining: Frame form, frame size, signal encoding, transmission rate, etc.
- Data link is the physical characteristics of communication.

- Ethernet standards can be grouped into 2 families: 802.3 and 802.2.

**802.2** covers the logical link and control functions - software components and upper layer interactions.

**802.3** covers the media access control functions of the data link layer as well as the physical layer - Hardware / physical components.

- 802.3 family has multiple specifications to support different types of media, fibre optical copper media, and different capabilities and transmission rates.
- Data encapsulation remains the same in addressing scheme and basic framing format.
- Media access control mechanism in 802.3 means we have collisions - Ethernet implements CSMA/CD for wired networks.

## Ethernet Operations / Collisions

- **Carrier Sense Multiple Access w/ Collision Detection (CSMA/CD)**
- Based on the nodes / devices to listen to media if any data being carried on media.
- Nodes can listen to the signal being carried on the media to detect if data is present.

- **Multiple Access** refers to the mechanism implemented in shared mediums where nodes are competing to use the transmission.

- **Collision detection** will listen to the media and only send data to media when free.

- Collisions still occur if 2 nodes / devices decide to send data at the same time. When the node detects collisions, they will stop sending data and wait for random periods to resend the data. (Time to Live?)

## CSMA/CD

- Node listens to the wire / shared medium.
- Hear signals currently being transmitted and detect noise or data with amplitude.
- If no transmission is happening, it will begin to send if media is idle.
- However, if 2 nodes send data at the same time / earlier but signal hadn't arrived.
- Collisions still occur and the nodes will stop sending down.
- An amplitude of the signal on media will increase as they listen to a jumbled signal.
- A jamming signal is set to continue the collision to ensure both devices / nodes can detect the collision and so they can stop and retransmit.
- The nodes / device will pause (backoff) for a period of time (random) and will start sending data again.

## CSMA/CA

- Wireless networks is defined by 802.11 standard due to the fact it is prone to radio waves and transmitters within a coverage area can begin to send media.

## Ethernet Operations / CSMA/CA

- Listen to media to see if data is being sent.
- If busy, it will back off and wait for a random period of time.
- When media is clear, they send a notification / request of intent.
- A node / controller (access point) will decide if the data will be sent or notification.

- Controllers in CSMA/CA will make sure no two devices will send data at the same time by implementing the request and clear mechanism so if the media is free, no two nodes will start to send data at the same time.
- Reduces the chance of collisions but still possible.

## MAC Address: Ethernet Identity

- Nodes need to identify themselves when sending a message and the receiver of message.
- By adding addressing in the nodes or include into layer 2 frame to identify the sender and receiver.

- In Ethernet it uses MAC address to identify nodes in communication.
- MAC address 48-bit binary value, written in 12 hexadecimal digits.

- Each network interface manufacturer has a unique Ethernet MAC address (unique 48-bit value).
- Regulated by IEEE requiring vendors and their manufacturing network interface to follow certain rules.

- Every vendor is assigned OUI - Organisation Unique Identifier.
- The 1st 3 bytes of every network interface card.

## MAC Address: Ethernet Identity (continued)

- Last 3 bytes are the unique NIC (Network Interface Card) identifier.
- Unique combination in the last 3 bytes ensured by the vendor.

[Diagram]

## Ethernet Frame -Ethernet Encapsulation

- Protocols evolve with time to adapt to faster speeds and technologies as Ethernet becomes the predominant layer 2.
- Early versions of Ethernet was slow at 10 MB/s but now Ethernet has been made to 10 GB/s → 100 GB/s → 200 GB/s → 400 GB/s.

- The data link layer has remained identical for all speed specifications.
- Since the frame structure has been defined by 802.3 standard.

- **Ethernet II (2)** - Frame format is used in TCP/IP networks.
- **IEEE 802.3** -Standard convention of frame structures.

## Fields Defined by Ethernet Protocol

- **Preamble:** Synchronization signal placed at the beginning of the frame. Allow both receivers to synchronize the clock to listen to the proper transmission rate.

- **Addressing Field:** Destination address and source address (MAC address).
  - **Destination Address:** Who is receiving the frame?
  - **Source Address:** Who is sending the frame?

- Destination address is at the front of source address as the receiver can read and recognize quicker if it's meant for them or else be discarded, like a person receiving a letter.

- **Type Field** - What network protocol carried in the data payload in layer 3 in communication.

- **Frame Check** - Check correctness of the data in the frame by introducing error checking values with frame check sequences (FCS).

- **Data Field** - Data carried / packet in frame. (Size of 46-1500 bytes maximum).
  - Maximum is required so no one frame is using media too long, so nodes aren't constantly sending data and taken.
  - Minimum is required due to collision detection. Ethernet is specified by maximum cable length, minimum cable length was designed to cater to 2 nodes at the maximum cable length to detect each other's data.

*Nodes need to listen to collisions. If a packet were small, the time to detect wouldn't be possible. To ensure the frame is long enough to continue to listen and notice the collision.*

The restriction on data requirements are no longer valid or needed since it's irrelevant but setup for backwards compatibility so it is kept.

# Ethernet Encapsulation -Frame

## IEEE 802.3

[Diagram]

## Ethernet II (2)

[Diagram]

# Ethernet MAC -Unicast MAC Address

- Communication between 2 nodes, 1 sender + 1 receiver: Layer 2 frame will contain the unique MAC address of the host and the source MAC of the sender.

# Layer 2 Frame Example

[Diagram]

- Unicast IP and MAC address are used to source and forward a packet and only possible if they are within the same Ethernet network.
- Source identifier also added to identify the sender via the source MAC and IP.
- If device A sends a message to device B, a unicast address ensures that it addresses to a specific device directly like a letter.

# Broadcast MAC Address

- Broadcast is the intent of sending data to all host / nodes on the network.
- To ensure that all nodes receive the data, the universal broadcast address: `FF-FF-FF-FF-FF-FF`.

- Destination IP address is the broadcast IP address for the layer 3 network.

- Both broadcast IP and MAC are used to forward a packet to all nodes in the network.