---
tags: [networking, wireless, security, week-9]
week: 9
aliases: [WPA, WEP, Wireless Security, Evil Twin, Rogue AP, 802.1x, RADIUS]
---

# Wireless Security

> **Source:** [[Week 9b]]

## Why Wireless Security Is Critical

Unlike wired networks where an attacker needs physical access to a cable, **anyone within radio range can receive wireless frames**. The open nature of the medium makes wireless inherently more exposed.

Core threats:
- **Interception** - attackers can passively capture all unencrypted wireless traffic
- **Rogue access points** - unauthorised APs that capture data or enable man-in-the-middle attacks
- **Denial of service** - disrupting wireless communication through flooding or spoofing management frames

---

## Wireless Attack Types

### Evil Twin Attack

A **man-in-the-middle** attack where an attacker sets up a rogue AP with the **same SSID** as a legitimate network.

1. Attacker places a rogue AP (often with stronger signal) near the target
2. Clients associate with the rogue AP, thinking it's the real network
3. All traffic passes through the attacker - credentials, session tokens, unencrypted data
4. Common in public spaces (cafés, airports, hotels)

> [!warning] An evil twin is difficult to detect visually - the network name looks identical. The attacker may even forward traffic to the real AP so users don't notice.

### Rogue Access Point

An unauthorised AP connected to the **wired network** without permission:
- Can be installed by an attacker or an unknowing employee
- Gives an attacker access to the internal network wirelessly
- Can capture data and MAC addresses
- Mitigation: monitor the radio spectrum for unauthorised APs (wireless intrusion detection)

### Denial of Service (DoS) Attacks

| Attack Type | Method |
|-------------|--------|
| **Management Frame Spoofing** | Attacker sends spoofed **disassociate** or **deauthentication** frames, forcing clients to disconnect repeatedly |
| **CTS Flood** | Attacker abuses CSMA/CA by repeatedly sending Clear-to-Send frames, reserving the medium and blocking all other clients from transmitting |
| **Accidental Interference** | Microwave ovens, cordless phones, or other APs on the same channel - not malicious but same effect |

> [!note] 2.4 GHz is significantly more vulnerable to interference than 5 GHz, both from other Wi-Fi and from non-Wi-Fi devices.

### War Driving

Driving or walking through an area scanning for Wi-Fi networks - looking for networks with weak or no security to exploit. Hiding the SSID is not an effective defence.

---

## Wireless Authentication Standards

Authentication has evolved significantly as weaknesses in older standards were discovered:

### WEP - Wired Equivalent Privacy *(Obsolete - do not use)*

- First generation wireless authentication
- Uses RC4 encryption with static keys
- Cryptographically broken - can be cracked in minutes with widely available tools
- **Never use WEP**

### WPA - Wi-Fi Protected Access

Introduced as an emergency replacement for WEP while WPA2 was being developed:
- Uses **TKIP** (Temporal Key Integrity Protocol) encryption
- Includes **MIC** (Message Integrity Check) to detect frame tampering
- Also considered deprecated - prefer WPA2 or WPA3

### WPA2 / WPA3

Current standards. Two modes:

| Mode | Who It's For | Authentication |
|------|-------------|----------------|
| **Personal (PSK)** | Home / SOHO | Pre-shared key (password) |
| **Enterprise** | Organisations | Per-user credentials via **RADIUS** server |

### Enterprise Authentication (802.1X + RADIUS)

- **RADIUS** = Remote Authentication Dial-In User Service
- Each user has their own credentials - not a shared password
- Uses **802.1X** standard for port-based network access control
- Even if one user's credentials are compromised, other users are unaffected
- Provides per-user accountability and the ability to revoke individual access

```
Client --→ Authenticator (AP) --→ RADIUS Server (Authentication)
                                        ↓
                              Accept / Reject
```

---

## Best Practices

- Use **WPA2-Enterprise** or **WPA3** where possible
- Never use WEP
- Force all non-trunk switch ports to access mode - prevent rogue APs from trunking into the network (see [[VLAN Security]])
- Monitor the radio spectrum for unauthorised APs
- Use separate VLANs for guest wireless and internal wireless (see [[VLANs]])
- Do not rely on SSID hiding as a security control - it is trivially bypassed

---

## Related Notes

- [[Wireless Networking]] - wireless fundamentals, architecture, and how access works
- [[VLAN Security]] - port security and hardening that complements wireless security
- [[VLANs]] - guest network segmentation via VLANs
