---
tags: [networking, reference]
---

# Protocol & Standard Quick Reference

## IEEE Standards

| Standard                | Layer                      | Covers                                       |
| ----------------------- | -------------------------- | -------------------------------------------- |
| **802.2**               | Data Link (LLC)            | Logical Link Control                         |
| **802.3**               | Data Link (MAC) + Physical | Ethernet (all speeds)                        |
| **802.3u**              | -                          | Fast Ethernet (100 Mb/s)                     |
| **802.3z**              | -                          | Gigabit Ethernet (fibre)                     |
| **802.3ab**             | -                          | Gigabit Ethernet (copper)                    |
| **802.11**              | Wireless                   | Wi-Fi base standard (CSMA/CA)                |
| **802.11a/b/g/n/ac/ad** | Wireless                   | Wi-Fi variants (5GHz / 2.4GHz / dual-band)   |
| **802.15**              | Wireless                   | Bluetooth                                    |
| **802.16**              | Wireless                   | WiMAX                                        |
| **802.1q**              | Data Link                  | VLAN tagging                                 |
| **802.1D**              | Data Link                  | Classic STP                                  |
| **802.1w**              | Data Link                  | Rapid STP (RSTP)                             |
| **802.1X**              | Data Link                  | Port-based access control (used with RADIUS) |
| **802.3ad**             | Data Link                  | Link Aggregation (LACP)                      |

---

## Key Protocols

| Protocol | Layer | Purpose |
|----------|-------|---------|
| **ARP** | 2/3 boundary | Resolves IP → MAC |
| **IPv4** | 3 | Packet routing |
| **IPv6** | 3 | Packet routing (next-gen) |
| **TCP** | 4 | Reliable transport |
| **UDP** | 4 | Unreliable transport |
| **DNS** | 7 | Hostname → IP resolution |
| **DHCP** | 7 | Automatic IP assignment |
| **OSPF** | 3 | Link-state routing protocol |
| **EIGRP** | 3 | Cisco routing protocol |

---

## Media Access Control Methods

| Method | Standard | Used By |
|--------|----------|---------|
| **CSMA/CD** | 802.3 | Wired Ethernet |
| **CSMA/CA** | 802.11 | Wi-Fi |
| **Token passing** | FDDI | Legacy token ring |

---

## Frame Forwarding Methods (Switches)

| Method | Waits For | Latency | Reliability |
|--------|-----------|---------|-------------|
| Store & Forward | Full frame + CRC check | High | Best - no corrupt frames forwarded |
| Cut-Through | Destination MAC only | Lowest | Risk of forwarding corrupt frames |
| Fragment-Free | First 64 bytes | Middle | Catches most collision errors |

---

## STP Port States

| State | Forwards Data | Learns MACs | Processes BPDUs |
|-------|--------------|-------------|-----------------|
| Disabled | No | No | No |
| Blocking | No | No | Yes |
| Listening | No | No | Yes |
| Learning | No | Yes | Yes |
| Forwarding | Yes | Yes | Yes |

**RSTP States**: Discarding / Learning / Forwarding

---

## STP Port Costs (IEEE)

| Link Speed | Cost |
|------------|------|
| 10 Gb/s | 2 |
| 1 Gb/s | 4 |
| 100 Mb/s | 19 *(default)* |
| 10 Mb/s | 100 |

---

## EtherChannel Protocol Modes

| Protocol | Mode | Behaviour |
|----------|------|-----------|
| LACP | Active | Initiates negotiation |
| LACP | Passive | Waits for negotiation |
| PAgP | Desirable | Initiates negotiation |
| PAgP | Auto | Waits for negotiation |
| Either | On | Forces channel, no negotiation |

---

## Related Notes

- [[IP Address Ranges Quick Reference]]
- [[🏠 Home]]
