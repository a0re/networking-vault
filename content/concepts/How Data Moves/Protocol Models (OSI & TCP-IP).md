---
tags: [networking, layer-model, week-1]
week: 1
aliases: [OSI Model, TCP/IP Model, Layered Model]
---

# Protocol Models (OSI & TCP/IP)

> **Source:** [[Week 1]]

## What Is a Network Protocol?

A protocol defines communication between two devices through a common standard. It specifies:
- How messages are **formatted and structured**
- **Rules** for communication (who speaks when, how errors are handled)
- **Encapsulation** - how addressing, headers, and data are packaged
- **Acknowledgement** - how send and receive is confirmed

---

## TCP/IP Model

The Internet model in common use. An **open standard** usable by any vendor.

| Layer | Name | Function |
|-------|------|----------|
| 4 | Application | Represents data to the user; encoding & dialog control |
| 3 | Transport | Communication between diverse devices across networks |
| 2 | Internet | Best path selection |
| 1 | Network Access | Controls hardware devices and physical media |

---

## OSI Model (Reference Model)

A more detailed 7-layer model used as a **reference** - it describes networking processes generically rather than implementing them directly.

| Layer | Name | Type |
|-------|------|------|
| 7 | Application | Software |
| 6 | Presentation | Software |
| 5 | Session | Software |
| 4 | Transport | Software |
| 3 | Network | Software |
| 2 | Data Link | Hardware boundary |
| 1 | Physical | Hardware |

> [!tip] The Data Link Layer is the boundary between software and physical hardware.

---

## Why Layered Models?

- **Standardisation** - enables competition between vendors; any compliant device works
- **Focused design** - each layer handles a specific, well-defined task
- **Independence** - changing one layer doesn't require changes to others
- **Evolvability** - e.g. upgrading physical media doesn't touch the application layer

---

## Protocol Model vs Reference Model

| Type | Purpose |
|------|---------|
| Protocol Model | Describes an actual implementation (e.g. TCP/IP) |
| Reference Model | Generic description of networking processes (e.g. OSI) |

---

## Related Notes

- [[Data Encapsulation]] - how each layer wraps the layer above's data
- [[Data Link Layer]] - the Layer 2 boundary in detail
- [[Physical Layer]] - Layer 1 encoding and transmission
