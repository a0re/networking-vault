# Link Aggregation

Link aggregation allows creation of a logical link made up of several links. EtherChannel is a form of link aggregation used in switched network. Layer 2 loops don't occur when trying to aggregate connections.

## Advantages

- Per-channel configuration as opposed to per-interface configuration.
- No need for network upgrade, more interfaces to upgrade / aggregate bandwidth.
- Load balancing.
- STP sees a logical link (Faster and simpler convergence).
- Redundant link going down won't have any impact.

## Restrictions of EtherChannel

- Logical link with higher bandwidth is still multiple links of a lower bandwidth.
- Single Ethernet frame can be transmitted on one physical link.
- Full throughput only happens when enough packets are queued on all physical connections concurrently.
- Serialization relay is the same as a single connection.
- Faster wire speed and less of a delay.
- Interface types cannot be mixed.
- EtherChannel provides full-duplex bandwidth.
- EtherChannel can consist up to 16 Ethernet ports.
- Cisco groups multiple physical ports to one to many logical EtherChannel.

## Link Protocols

### PAgP

- Cisco proprietary.
- Cannot use with other equipment.
- Do not use.

**PAgP Modes:**

- **Desirable:** Actively ask to participate.
- **Auto:** Passively wait for connection.
- **On:** No negotiation.

### LACP Modes

- **Active:** Actively ask if other side can participate.
- **Passive:** Waiting for other side.
- **On:** Channel member without negotiation.

## Configuration Guidelines

- Both switches must support EtherChannel.
- Speed and duplex must match.
- VLAN match - interface range VLAN.
- Establish trunk operation.
- Shutdown one side of connection.
- Aggregate ports in a channel.
- Enable shutdown ports.