---
tags: [networking, reference, stp]
---

# STP Port Cost Table

Used by the Spanning Tree Algorithm to determine the best (cheapest) path to the root bridge.

## IEEE Port Costs

| Link Speed | STP Cost |
|------------|----------|
| 10 Gb/s | 2 |
| 1 Gb/s | 4 |
| 100 Mb/s | **19** *(default in most networks)* |
| 10 Mb/s | 100 |

Lower cost = preferred path.

## How Path Cost Is Calculated

Path cost is the **sum of all port costs** from a non-root switch to the root bridge.

**Example:**
```
Switch B → Switch A (root)
  Link: 1 Gb/s (cost 4)
  Path cost from B to root = 4

Switch C → Switch B → Switch A (root)
  Link C→B: 100 Mb/s (cost 19)
  Link B→A: 1 Gb/s (cost 4)
  Path cost from C to root = 19 + 4 = 23
```

## PVST+ Extended Bridge Priority

With PVST+, the 16-bit Bridge Priority field is split:

| Field | Bits | Notes |
|-------|------|-------|
| STP Priority | 4 bits | Multiple of 4096 |
| Extended System ID (VLAN ID) | 12 bits | 0–4095 |

**Available priority values**: 0, 4096, 8192, 12288, 16384, 20480, 24576, 28672, 32768 (default), 36864, 40960, 45056, 49152, 53248, 57344, 61440

## Common Priority Configurations (Cisco)

| Command | Result | Priority Value |
|---------|--------|----------------|
| `spanning-tree vlan X root primary` | Sets to 24576 | Makes this the root if default priority is 32768 |
| `spanning-tree vlan X root secondary` | Sets to 28672 | Backup root |
| Default | 32768 | No preference |

## Related Notes

- [[Spanning Tree Protocol (STP)]] - full STP explanation
- [[STP Advanced (PVST+ & RSTP)]] - PVST+ and Rapid STP
