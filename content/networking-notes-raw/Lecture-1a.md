# Network Protocol

- Defines communication between two devices through a common standard.
- Message formatted and structured.
- Rules of communication.
- Message formatting and encapsulation.
  - Addressing protocol.
  - Headers.
  - Data.
- Protocol defines acknowledgement with send and receive.

[Insert Image of Packet]

*Note: Headers are placed before the data to include addressing formation (encapsulation) and split messages.*

# Layered Protocol Models

## TCP/IP Layered Architecture Model

- Simplified communication standard between layers.
- Process of sending through a network requires multiple protocols.
- Layer-specific protocol.
- Internet model defines and describes function and task in network.
- Open standard and used by any vendor.

## Layers of TCP/IP

- **Application** -Represents data to the user and encoding and dialog control.
- **Transport** - Communication between diverse devices across networks.
- **Internet** - Best path.
- **Network** - Controls hardware devices and media.

**Reference Models**

- A generic networking processes description.

# Data Link Layer

- Layer 2: Bound between software and physical components.
- The upper layers in software to access the media.

- Data link accepts "packets" produced by the network layers 3 protocols and are packaged into "frames" / PDU.

[Insert Image of OSI Model / TCP/IP Model]

**Software Related**

- Application.
- Presentation.
- Session.
- Transport.
- Network.

**Hardware Related**

- Data link.
- Physical.

Frame format that is correct so it can be transmitted the type of media.

**WTF is this?**

Data link layer - Frame and encapsulates Layer 2 protocol? (Layer 2)

Example: Allow a computer to communicate over different types of media. The frame data for the right type of media and controls the media access control.

**Protocol Model:** Model describes an actual implementation.

[Insert Image of OSI Model / TCP/IP Model]