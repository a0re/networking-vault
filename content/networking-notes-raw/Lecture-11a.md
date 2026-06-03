# Transport Layer
- The main role of the transport layer is to transport communication between 2 networked devices, ip layer enables network connectivity but the transport layer is the one connecting source and destination applications 
- When an application at the source host generates data all the applications needs to know is the name of the destination host and which application is running on the destination host
- Application use names rather than destination ip's while the DNS takes care of the translation from the name to the ip address
- The application has no regards to the host type, media type sent, routing taken, congestion in the network layer etc.
- Transport layer is in charge making this link between application and network
- In certain cases it will control the transmission setting for network changing capabilities 


- Track individual or task performed: conversation between source application and destination application (web browser to a website)
- Segement and reassemble data - when application at the start host generate a data stream the data layer needs to prepare to send across the network and made into a proper segement size to carry across the network in the payload of a layer 3 packet
- The Destination the receive and transport layer will re assemble the data stream in the layer of the receiving host 
- Uniquely identify each application and conversation concurrently happening between app using source and destination port (TCP/UDP) 

- Primary role Allows application to communicate between different categories applications (or transport layer protocols) 


- There are applications that require fast tranmission with low overhead with priority to send data receives a real time transmission?
- This type of transmission wouldn't want a lot of control mechanisms or overhead - checksum or acknowledgement data since it would slow down the type of connection
- Real Time transmission should always priortize fast transmission over reliability 
- Reliable connections would be desireable to have Reception acknowledgement to retransmit the data, meaning more overhead with more controlled fields to transport layer fields 


- 2 Distinct transport layer protocol - UDP (user datagram protocol) required often real time transmission
- TCP (Transmission control protocol) - Reliable connection
- The chosen protocol between UDP and TCP has an impact on how an application performs under network conditions

UDP doesn't guarnatee that the data will be received but still implements tracking of conversation and use port and src and destination 
It segements the data stream (Datagram) to appropriate sizes segements when used - no additional overhead (Src and destination port in layer 4 header to identify)

TCP/IP increases overhead and complicity to segements meant - the header is just like UDP with added acceptance knowdedgement and data tracking mechansims

[Claude gimme an example where UDP and TCP/IP Would be helpful]

depends on the requirements of the payload and the traffic being carried
Other transport layer protocols do exist.

Both TCP & UDP performs port numbering each layer 4 segement will have a src and destination port number in the header and how they are identified

Port numbering each segement allows network host to concurrently handle conversations and application to use the same infrastructure

Different application are assigned different port number at source and destinatinon host and inserted at the layer 4 header

The server side application usually assign a well known port or a registered port 
port 80 for http servers
port 1110 pop3 
port 531 Internet Message (IM)

Client applications are often random within a particular range f

Transport layer supports concurrent applications with a tuple 

Tuple: A pair of a Source and destination IP's and port's 
Uniquely identifers single streams of communications

Server side ports are standardized while client side is randomly generated

Destination port | source port 

Port number diagram
range 106535

Insert table about port numbers

UDP ports

There are combined ports for TCP and UDP   