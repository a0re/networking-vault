---
tags: [networking, week-9, wireless]
week: 9
---

# Week 9 (continued) - Wireless Networking

## Topics Introduced This Week

- [[Wireless Networking]] - 802.11 standards, access point types, wireless architecture (BSS, ESS), SSID, association process
- [[Wireless Security]] - Evil twin attacks, WEP/WPA, rogue access points, DoS attacks

> Week 9 also covered [[Link Aggregation (EtherChannel)]] - see that note for the wired side.

## Key Concepts

**Wireless uses CSMA/CA**, not CSMA/CD - you can't detect collisions on a radio medium, so you avoid them instead. The **hidden node problem** (two clients can't sense each other but both reach the AP) is addressed by the **RTS/CTS** mechanism.

**BSS vs ESS**: a single access point creates a Basic Service Set. Multiple overlapping APs sharing the same SSID form an Extended Service Set - cells should overlap by 10–15% to allow roaming without dropping connection.

**Wireless is inherently insecure** compared to wired - anyone in range can receive frames, rogue APs can impersonate legitimate ones, and the unlicensed 2.4 GHz band is prone to interference from microwaves and cordless phones.

## Connections to Other Weeks

- VLANs can segment wireless networks - see [[VLANs]] (Week 3)
- CSMA/CA was introduced alongside CSMA/CD in [[Data Link Layer]] (Week 1–2)
