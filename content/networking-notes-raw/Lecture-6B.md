# IPv4 -Routing Between Networks

- Routing protocols exchange their own information, they can to build other router entries and networks while switches and PCs don't.

- The host device just requires the default gateway locally because it will rely on the router's knowledge to communicate.

- Router exchanges routing information to get to forward packets to get the information.

- For computers to communicate externally, they must know the local gateway IP.

## Gateway Required

- Not required if they are from the same switch.
- Not required if they have the same network portion / broadcast domain.
- Able to figure the ARP reply request.
- No means to figure out MAC address.

- Gateway required if they don't share broadcast domain.
- The host trying to figure the MAC address outside its network will require the router's MAC address.

- Router will look at destination IP (Layer 3) to forward to the requested host.
- Routers will change destination MAC address.

- Since the router shares the same broadcast domain, it can communicate between both networks.
- The destination address needs to be determined by the host if it is in the subnet range or not.

# IPv4 -Routing

## Default Gateway

- Hosts has their own routing table to direct to the right destination.
- Local tables contains:
  - **Direct connection** - Local IP.
  - **Local network route** - Local network address.
  - **Local default route** - Other networks.

- All hosts need a gateway address if they need to communicate outside of its local link layer network.
- Switches are configured with management IP address.
- EIGRP? What is that.
- **Local Link (L)** - IP addresses associated with the interface.
- **Directly Connected (C)** - Directly connected network address.

### Pathing

Devices initial packet:

- SRC IP + MAC.
- Destination IP.
- Destination MAC.

(If same subnet / host portion) Broadcast in local network?
If different subnet, MAC of default -router.
Decide to forward to default gateway.

### Packet Traversal

- IPs stay the same.
- Remove the old layer 2 header.
- MAC changes at each hop.
  - Source MAC - Router out interface.
  - Destination MAC - Next hop MAC.
  - If it passes a switch, the src MAC doesn't change.