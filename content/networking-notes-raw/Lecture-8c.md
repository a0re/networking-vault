# Spanning Tree Protocol - Advanced

## PVST+ (Per-VLAN Spanning Tree)

- **STP - 802.1D Standard:** Builds one whole spanning tree, slow convergence.
- **PVST+ - Cisco Standard:** Different spanning tree per VLAN, slow convergence.
- **RSTP - 802.1w Standard:** Build 1 spanning tree for all VLANs, fast convergence.
- **RPVST+ - Cisco Standard:** Builds spanning tree for each VLAN, fast convergence.

- STP IEEE standard is 802.1D.

## Rapid PVST+

- Preferred protocol in modern day to prevent layer 2 loops.
- No blocking ports - Port states: discarding, learning or forwarding.
- Supports new port type - Alternate port discarding state.
- RSTP (802.1w) supersedes STP (802.1D) and is also backwards compatible.
- Rapid PVST+ is independent instance of RSTP for each VLAN.

## Edge Ports

- Ports that will never be connected to it.
- Immediately transition to forwarding, function similarly to portfast.

## Link Types

- Determine whether ports can be transition to forwarding state.
- Edge ports connection and port-to-port connection are candidates for rapid transitions to forwarding state.

## Spanning-Tree BPDU Guard Enable

- BPDU: Sets port to error-disabled state when a BPDU is received.

## Spanning-Tree Portfast

- Blocks port transition from blocking to forwarding state immediately.

## Load Balancing

[Missing]

---

- STP only maintains 1 spanning tree for 1 entire network.

- PVST+ runs an independent IEEE 802.1D STP instance per VLAN:
  - **Benefits:**
    - Utilize unused links.
    - Better load balancing of traffic.
  - **Cons:**
    - More CPU usage to calculate.
    - More bandwidth lost for each BPDU per VLAN.

- Possible to lower balance traffic across links and ports with PVST+ and moderate traffics of VLANs in trunk.

## Extended System ID

- PVST extended bridge priority with system ID (VLAN) included:
  - Extended ID to each switch to have a unique Bridge (BID) for each VLAN.
  - New VLAN ID leaves fewer bridge priority available.
  - Uses the multiple of 4096.

| STP Priority | Extended ID | Bridge Priority |
| ------------ | ----------- | --------------- |
| 4 Bits | 12 Bits | 16 Bits |

- Default bridge priority - VLAN 1 = 32769 (PVST) / 32768 (STP).

### Configuring Priorities

```
Spanning-tree VLAN 1 - Root Primary - Set priority to 24576
Spanning-tree VLAN 1 - Root Secondary - Set priority to 28672
```

`Show spanning-tree`

- Confirm by - this bridge is the root: 24577 (Priority 24576).