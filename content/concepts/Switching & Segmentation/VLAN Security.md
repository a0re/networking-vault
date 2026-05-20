---
tags: [networking, vlan, security, layer-2, week-3]
week: 3
aliases: [Port Security, MAC Flooding, MAC Spoofing, VLAN Hopping, Blackhole VLAN]
---

# VLAN Security

> **Source:** [[Week 3]]

## Default Behaviour (Risk)

Switch ports are **enabled by default**. Access ports pre-patched to public or semi-public outlets mean any user plugging in a device immediately gets network access on that VLAN.

Port security is **disabled by default** on Cisco switches.

---

## Layer 2 Attack Types

### MAC Address Flooding
- An attacker sends frames with many **fake source MAC addresses**
- The switch's CAM table fills up (limited memory)
- Once full, the switch can't learn new MACs and **floods all frames to all ports** (like a hub)
- The attacker can now capture all traffic on the segment

**Mitigation**: [[#Port Security]] - limit MAC addresses per port.

### MAC Spoofing
- An attacker changes their NIC's source MAC to **impersonate a legitimate host**
- If they spoof the **default gateway's MAC**, traffic from other hosts gets redirected to the attacker
- The attacker can sniff the packets or perform man-in-the-middle attacks

**Mitigation**: Port security with static MAC binding.

### VLAN Hopping (Trunk Negotiation Attack)
- By default, DTP is set to **Dynamic Auto** - a switch will accept a trunk request
- An attacker can send DTP frames to negotiate a trunk with the switch
- Once trunked, the attacker gains access to **all VLANs**
- An attacker can also craft double-tagged 802.1q frames to jump VLANs

**Mitigation**: Force all non-trunk ports to access mode; disable DTP on user-facing ports.

---

## Hardening Best Practices

### Unused Ports
- Move unused ports to a **Blackhole VLAN** - an isolated VLAN that carries no user or management traffic
- Administratively **shut down** unused ports

### Management VLAN
- Do **not** use VLAN 1 for management - it carries Layer 2 control traffic by default, which exposes topology information
- Create a dedicated management VLAN

### Trunk Port Configuration
- Only designated inter-switch links should be trunk ports
- Force them explicitly: `switchport mode trunk` + `switchport nonegotiate`
- Force all other ports: `switchport mode access`

### Summary Table

| Port Type | Recommended Config |
|-----------|-------------------|
| User-facing (in use) | `switchport mode access` |
| Unused | `switchport mode access` + shutdown + blackhole VLAN |
| Inter-switch links | `switchport mode trunk` + `nonegotiate` |

---

## Port Security

Limits the number of MAC addresses allowed on a port and defines what happens when the limit is violated.

### MAC Address Learning Modes

| Mode | Behaviour |
|------|-----------|
| **Static** | Administrator manually programs allowed MAC addresses |
| **Dynamic** | Currently learned MACs (lost on reboot) |
| **Sticky** | Switch automatically adds learned MACs to the config (persistent) |

### Violation Actions

| Mode | Behaviour |
|------|-----------|
| **Protect** | Drops frames from unknown MACs; valid MACs still pass; no alert |
| **Restrict** | Same as Protect, but increments a violation counter |
| **Shutdown** | Port goes to **err-disabled** state; requires manual intervention to restore |

### Default Port Security Settings (Cisco)

| Setting | Default |
|---------|---------|
| Port Security | Disabled |
| Max MAC Addresses | 1 |
| Violation Mode | Shutdown |
| Sticky Address Learning | Disabled |

> [!tip] **Sticky + Shutdown** is a common production configuration: let the switch learn the first MAC automatically, then lock the port down if anything else shows up.

---

## Related Notes

- [[VLANs]] - VLAN concepts and trunk configuration
- [[Collision Domains & Switching]] - how the MAC table works (what MAC flooding exploits)
- [[Data Link Layer]] - Layer 2 fundamentals
