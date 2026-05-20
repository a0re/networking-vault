# Layer 2 Redundancy

- Network availability - Downtime five nines uptime 99.999% -5.25 minutes down.
- In order for network to be fault tolerant to not rely on a single switch interface or device.
- Redundancy is important to be fault tolerant.
- What is the balance of cost redundancy and network availability.

**AIM:**

- Minimize downtime.
- Minimize points of failure.
- Fault tolerance.

## Broadcast Storms

- The risk is involved when multiple paths exist in layer 2 loops.
- Ethernet frames does not have Time-To-Live (TTL).
- Broadcast frames are forwarded to all switch ports and create an endless loop since the mechanism of time-to-live (TTL) wasn't created.
- Bandwidth disrupts normal traffic flow.
- All devices use up processes for the broadcast.
- MAC address table instability -false information.

## Unicast Duplication

- Unicast frames sent on a loop network receives duplicated frames at destination device.
- Upper layer protocols aren't designed to recognize duplicated transmission.
- TCP also results in a slowdown on transmission rate.