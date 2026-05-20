---
tags:
  - networking
  - wireless
  - csma-ca
  - week-9
week: 9
aliases:
  - Wireless
  - WiFi
  - 802.11
  - WAP
  - Access Point
  - BSS
  - ESS
  - SSID
  - CSMA/CA
---

# Wireless Networking

> **Source:** [[Week 9b]]

## Why Wireless?

Wireless networking removes the physical constraint of cabling - users can roam freely. This introduces three core concerns not present in wired networks:

| Concern | Why It Matters |
|---------|---------------|
| **Coverage Area** | Where to place access points to avoid dead zones |
| **Interference** | Shared unlicensed spectrum means other devices compete |
| **Security** | Anyone in radio range can receive frames - no physical access needed |

---

## IEEE 802.11 Standards

All Wi-Fi is based on the **IEEE 802.11** standard family, which uses **CSMA/CA** for media access.

| Standard | Band | Notes |
|----------|------|-------|
| 802.11a | 5 GHz | Early, less interference |
| 802.11b | 2.4 GHz | Early, wide range, prone to interference |
| 802.11g | 2.4 GHz | Faster than b, backward compatible |
| 802.11n | 2.4 / 5 GHz | Dual-band, MIMO |
| 802.11ac | 5 GHz | High throughput (Wi-Fi 5) |
| 802.11ad | 60 GHz | Very short range, very high speed |

**Related standards:**
- **802.15** - Bluetooth (short-range personal area network)
- **802.16** - WiMAX (wide-area wireless)

> [!note] 2.4 GHz has longer range but is more congested and prone to interference from microwaves, cordless phones, and baby monitors. 5 GHz has shorter range but more available channels and less interference.

---

## Wireless Device Types

| Device | Role |
|--------|------|
| **Wireless Access Point (WAP)** | Provides the radio interface; bridges wireless clients to the wired network |
| **Wireless Router** | Combined WAP + Ethernet switch + router; common in homes |
| **Wireless NIC** | Client-side radio hardware in laptops, phones, etc. |

### SOHO (Small Office / Home Office) Setup

A single wireless router typically serves as all three: access point, switch, and router. Each WAP is configured individually - if multiple APs are placed too close together, they can interfere with each other.

A **VLAN** can be used to segment a wireless network (e.g. a guest Wi-Fi on a separate VLAN from the internal network). See [[VLANs]].

---

## CSMA/CA and the Hidden Node Problem

Wireless uses **CSMA/CA** (Collision Avoidance) because collision *detection* is impossible on a radio medium - a transmitting node can't hear collisions while sending.

### Hidden Node Problem

Two clients (PC1 and PC2) may both be in range of the access point but **out of range of each other**. Neither can sense the other's transmission, so both may transmit simultaneously → collision at the AP.

```
PC1 ←--- can't hear each other ---→ PC2
  \                                   /
   --------→  Access Point  ←--------
```

### RTS/CTS Solution

**Request to Send / Clear to Send** is a feature of CSMA/CA that resolves hidden node collisions:

1. Client sends an **RTS** frame to the AP
2. AP broadcasts a **CTS** frame, which all clients in range hear
3. All other clients defer - only the requesting client transmits
4. When transmission completes, other clients may send their own RTS

> [!tip] RTS/CTS adds overhead, so it's typically only enabled when hidden node problems are detected, not by default.

---

## Wireless Frame

802.11 wireless frames have **more control fields** than Ethernet frames, because the wireless medium requires more coordination (acknowledgements, power management, fragmentation, etc.). The frame format is more complex than a wired Ethernet frame.

---

## Wireless Network Architectures

### Ad Hoc Mode (IBSS)
- Devices connect **directly** to each other without an access point
- Example: tethering / personal hotspot
- Limited range and scalability

### Infrastructure Mode

Clients connect through an **access point**:

| Architecture | Description |
|-------------|-------------|
| **BSS (Basic Service Set)** | One access point + its associated clients |
| **BSA (Basic Service Area)** | The physical radio coverage area of one AP |
| **ESS (Extended Service Set)** | Two or more APs sharing the same SSID, enabling roaming |
| **ESA (Extended Service Area)** | The combined coverage area of all APs in an ESS |

### ESS and Roaming

When a single AP doesn't provide enough coverage, multiple APs are deployed with:
- The **same SSID** - clients see one network name
- **Overlapping coverage of 10–15%** - enough overlap for seamless roaming without a gap
- **Different channels** - non-overlapping channels (1, 6, 11 on 2.4 GHz) prevent APs from interfering with each other
- Each AP has a unique **BSSID** (its MAC address) so clients can identify which AP they're associated with

> [!important] The SSID is the human-readable network name. The BSSID is the MAC address of the access point - it's how clients and the system distinguish between APs in an ESS.

---

## SSID and Association

**SSID (Service Set Identifier)** - the network name clients connect to.
- 2–32 characters long
- Can be shared across multiple APs in an ESS
- Can be hidden (AP doesn't broadcast it) - but hiding is **not a security measure** (it's trivially discoverable)

### AP Discovery Modes

| Mode | How It Works |
|------|-------------|
| **Passive** | AP broadcasts **beacon frames** advertising SSID, security settings, supported standards - clients listen |
| **Active** | Client broadcasts **probe request** frames on multiple channels; APs that match respond |

### AP Association Process

1. **Probe** - client sends 802.11 probe request
2. **Authentication** - client authenticates with the AP
3. **Association** - client associates and is assigned resources

---

## Wireless Interference

The 2.4 GHz band is **unlicensed** - any device can use it. Sources of interference:
- Other Wi-Fi networks
- Microwave ovens
- Cordless phones
- Bluetooth devices

**5 GHz** has more available channels and less non-Wi-Fi interference, but shorter range through walls.

**Non-overlapping channels (2.4 GHz)**: 1, 6, and 11 - adjacent APs should use different non-overlapping channels to avoid co-channel interference.

---

## Related Notes

- [[Wireless Security]] - attacks on wireless networks and authentication methods
- [[Data Link Layer]] - CSMA/CA introduced here in context of media access control
- [[VLANs]] - used to segment wireless guest networks
