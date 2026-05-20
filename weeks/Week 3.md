---
tags: [networking, week-3]
week: 3
---

# Week 3 - VLANs & Layer 2 Security

## Topics Introduced This Week

- [[VLANs]] - VLAN concepts, access/trunk ports, tagging (802.1q), VLAN types, DTP
- [[VLAN Security]] - Layer 2 attack types, port security, blackhole VLANs, best practices

## Key Concepts

**VLANs** partition a physical switch into multiple logical broadcast domains. Traffic can only cross VLAN boundaries via a Layer 3 device (router). This is a key insight - Layer 2 is always VLAN-local.

**802.1q tagging** inserts a 4-byte tag into the Ethernet frame (12 bits for the VLAN ID) so trunk links can carry traffic for multiple VLANs simultaneously.

**Layer 2 attacks** exploit the trust model of switches: MAC flooding fills the CAM table, MAC spoofing impersonates legitimate hosts, and VLAN hopping abuses DTP to gain trunk access.

## Connections to Later Weeks

- VLANs require a router to communicate between them - covered in [[Inter-VLAN Routing]] (Week 7)
- Trunk ports and 802.1q tagging reappear in [[Inter-VLAN Routing]] (Router-on-a-Stick)
- Broadcast domain concepts connect back to [[Collision Domains & Switching]] (Week 2)
