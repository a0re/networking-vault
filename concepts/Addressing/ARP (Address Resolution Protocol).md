---
tags: [networking, arp, layer-2, layer-3, week-4]
week: 4
aliases: [ARP, Address Resolution Protocol, ARP Cache, ARP Table]
---

# ARP (Address Resolution Protocol)

> **Source:** [[Week 4]]

## The Problem ARP Solves

Layer 3 (IP) knows **where** to send a packet (destination IP). But Layer 2 (Ethernet) needs a **MAC address** to actually deliver the frame on the local segment. ARP bridges this gap.

> **ARP resolves a known IP address to its corresponding MAC address.**

Both are required because of the layered architecture - a router looks at IP addresses, a switch looks at MAC addresses. Within a local network, packets travel using Layer 2 MAC addresses.

---

## How ARP Works

ARP is a **Layer 2 broadcast** process - the request goes to all hosts on the local segment (`FF:FF:FF:FF:FF:FF`). It is encapsulated in an Ethernet frame (no IP header - it operates below IP).

### ARP Request & Reply Sequence

```
Host A (192.168.1.10) wants to reach Host B (192.168.1.20)
Host A doesn't know Host B's MAC address

1. Host A broadcasts: "Who has 192.168.1.20? Tell 192.168.1.10"
   → Destination MAC: FF:FF:FF:FF:FF:FF (all hosts receive this)
   → All hosts on the segment receive the broadcast

2. Host B responds with a unicast reply: "192.168.1.20 is at AA:BB:CC:DD:EE:FF"
   → Only sent back to Host A (unicast)

3. Both Host A and Host B store each other's IP↔MAC mapping in their ARP cache
```

---

## ARP Cache

The ARP cache (ARP table) stores recently resolved IP → MAC mappings **in memory**:

- Entries are stored by **both** the requester and the responder
- Cache entries **time out** after a period of inactivity (typically 4–20 minutes depending on OS)
- Entries can be manually cleared or will be removed for new entries when the cache is full

> [!tip] You can view the ARP cache on any machine:
> - **Windows/Linux/macOS**: `arp -a`

---

## Internal Network ARP (Same Subnet)

When Host A wants to reach Host B on the **same subnet**:

1. Host A checks its ARP cache - if a MAC is already cached, skip to step 4
2. Host A sends an ARP broadcast for Host B's IP
3. Host B replies with its MAC (unicast)
4. Host A sends the packet directly to Host B using the resolved MAC

---

## External Network ARP (Different Subnet)

When Host A wants to reach a host **on a different network**:

1. Host A determines the destination is outside its subnet (by comparing against its subnet mask)
2. Host A sends the packet to its **default gateway** (router)
3. Host A ARP-requests the **router's MAC address** (not the remote host's MAC)
4. The router receives the packet, strips the Layer 2 frame, looks at the destination IP, and forwards it toward the destination

> [!important] ARP is **always local** - a host never ARPs for a MAC address outside its own subnet. For external traffic, the target of the ARP is always the default gateway's IP.

---

## MAC Address vs IP Address Recap

| Property             | MAC Address                              | IP Address                           |
| -------------------- | ---------------------------------------- | ------------------------------------ |
| Layer                | Layer 2 (Data Link)                      | Layer 3 (Network)                    |
| Length               | 48 bits                                  | 32 bits (IPv4)                       |
| Format               | Hex (e.g. `AA:BB:CC:DD:EE:FF`)           | Dotted decimal (e.g. `192.168.1.1`)  |
| Assigned by          | NIC manufacturer (permanent)             | Admin or DHCP (logical)              |
| Scope                | **Locally significant** - per L2 segment | **Network-wide** - globally routable |
| Changes at each hop? | **Yes** - new MAC for each link          | **No** - stays the same end-to-end   |

---

## ARP & Routing Interaction

At every router hop:
1. The router ARPs for the **next-hop IP** (another router or the final destination)
2. Gets the next hop's MAC
3. Rebuilds the Ethernet frame with its own MAC as source and next-hop MAC as destination
4. Forwards the packet

This is why MAC addresses change at every hop but IP addresses do not.

---

## Related Notes

- [[Ethernet & MAC Addressing]] - MAC address structure and broadcast behaviour
- [[Network Layer (IP)]] - the Layer 3 context ARP bridges to
- [[IPv4 Addressing]] - subnetting that determines when ARP goes to the gateway
- [[IPv4 Routing]] - how routers forward after ARP resolves the next hop
- [[Collision Domains & Switching]] - ARP broadcasts and MAC table learning
- [[Inter-VLAN Routing]] - ARP behaviour across VLANs
