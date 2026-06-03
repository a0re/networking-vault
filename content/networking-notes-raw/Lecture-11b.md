UDP Protocol Format

connectionless porotcol where host won't check if it's online before sending data, no pre establish before sending data and also considered stateless and won't keep track of the conversation from the client or the server

Reliability mechanisms could be implemented in upper layer of the OSI model with no session establishment or data tracking

No reliable or return transmissions and won't provide ordered reassembling or sequence number in the header 
No Flow control in UDP 
Data will be sent at the source capacity with no regards of the destination preference

Because of the lack of these mechanisms it is considered very simple, and only need the source and destination the application in the conversation, 
Length field is to identify to specific the size of the layer 3 payload 
Checksum number for data integreity checks


DNS
Video Streaming
VoIP

Data reassmembly when sent can be disordered in udp it is sent over ip and layer 3 doesn't guarantee a conneciton between source and destination. if a data stream is broken into multiple datagrams 

Datagrams can take different routes and characteristics when sending to destination 

The transport layer can reassemble them in the wrong layer when passed on to the upper layer as there is no mechanism to track how things are sent 

What is a radius port? 