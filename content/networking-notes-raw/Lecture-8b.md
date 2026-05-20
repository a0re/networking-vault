# Spanning Tree Protocol (STP)

## BID / Bridge ID

[Diagram]

8 bytes:

- BID - 2 bytes.
- MAC address - 6 bytes.

Bridge ID - BID / Bridge Priority - 32,768.
MAC address - hexadecimal.

### Rules of Root Bridge

- Lowest Bridge ID is root.
- Otherwise, lowest MAC address is root.

## Root Port - Rules

- Lowest / cheapest path to root bridge.
- Lowest Bridge ID and port ID.
- Only one root port for each non-root bridge.
- Root port can send traffic.

## Designated Ports

- Each link must have a designated port.
- Other end of segment, root port or end with cheapest path to root bridge or Bridge ID.
- On root bridge, all ports are designated ports.
- Can send and receive traffic.

| Link Speed | Cost |
| --------- | ----- |
| 10 Gb/s | 2 |
| 1 Gb/s | 4 |
| 100 Mb/s | 19 (Default) |
| 10 Mb/s | 100 |

- Root bridge has been elected. STA determines the best paths to root bridge.
- Cost can be configured on individual ports and the path to destination root bridge.

### Step 3 - Elect Non-Designated Ports

- All ports on root bridge will be designated ports.
- All links connected to root port will be designated ports.
- Other link ports with lowest path costs to root bridge is designated, others are non-designated.
- Lowest BID is used in the tiebreaker.

## STP Convergence

Time it takes in the network topology:

- Determine switch to be the root bridge.
- Set switch ports to their final spanning-tree port.

**TL;DR**

- **Root Bridge:** Election - Lowest Root ID, Bridge ID and MAC.
- **Root Port:** Path cost to assign R.
- Shortest path to root bridge.
- **Designated Ports:** All ports from root bridge are designated ports.
- **Non-Designated Ports:** Hosts with lower Bridge ID will be designated or non-designated.

## Verification

`Show spanning-tree`

- Show Root ID, Bridge ID.
- Interfaces being root or FWD / BLK.

## Port States

- **Listening:** STP determining port can participate in frame forwarding BPDU.
- **Learning:** Preparing to participate in frame forwarding.
- **Forwarding:** Fully operational - both BPDU and data frames forwarded.
- **Disabled:** Not participating in STP and does not forward frames.

- STP ensures there is only 1 logical path between destinations on the network by blocking redundant paths.
- Ports are blocked when data is prevented from entering or learning that port.
- Doesn't include Bridge Protocol Data Unit (BPDU) used by STP to prevent loops.
- Physical paths still exist to provide redundancy but are disabled to prevent loops from occurring.
- If path is ever needed due to network cable or switch failure, STP recalculates paths and unblocks the paths.

## Spanning Tree Algorithm (STA)

- STP is determined by STA to determine which switchports to be blocked.
- Algorithm selects a single switch as the root for the reference point to all STA calculations.

It follows three steps:

1. Select 1 root bridge.
2. Select root port on non-root bridges.
3. Select designated and non-designated ports.

Steps for STP:

1. All switches in the broadcast domain are in the election process.
2. Switches boots and sends out BPDU frames with BID (Bridge ID) and current root ID.
3. Every BPDU the root ID adjusts itself in the switch.
4. Forward new BPDU frames with lower root ID.
5. Lowest BID ends up being identified as root bridge.
6. [Missing]