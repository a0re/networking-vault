# Wireless Networks

## 802.11 Wireless Frame

- Wireless has more fields to control compared to Ethernet.

## Hidden Node Problem

- PC1 and PC2 cannot sense each other, so when sending a packet, a collision occurs at the same time.

To resolve collisions detection / hidden node - Request to send / Clear to send (RTS/CTS).

- A feature of CSMA/CA (RTS/CTS).
- Allows a negotiation between a client and access point.

- When enabled, access points allocate the medium via CTS to requesting station for as long as required to complete transmission.
- When transmission is complete, other stations can request the channel via RTS.

## Ad Hoc Mode

- Tethering (personal hotspot) 802.11.
- Coverage area.
- Basic Service Area (BSA).
- When a smartphone or tablet with cellular data accesses infrastructure mode (Basic service set), 1 access point.
- (Extended Service Set) 2 or more access points.

## Extended Service Set

- When BSS is not enough, it will activate for RF coverage in ESS.
- 1 BSS is different from another by SSID identifier.
- BSSID, which is the MAC address of the access point.
- Coverage is extended by ESA (Extended Service Area).

- Overlap by 10%-15%.

## Common Distribution System

- ESS includes SSID to allow access point to access point connection.
- ESS should overlap between cells only 10%-15%.

## Association Parameters

- 802.11b interference to use non-overlapping channels 1, 6, 11.

- Access points must have second radio to operate in two RF bands (radio spectrum).

- Faster clients that do contend with the channel has to wait longer.

**SSID** - Service Set Identifier - Associated parameter.

- SSID can be shared between networks.
- SSIDs are 2-32 characters long.
- Used to connect to access points by devices.

## Passive Mode

- Advertise SSID via broadcast beacon frames.
- Security settings and supported standards.
- Allow wireless clients to learn networking service to local area.

## Active Mode

- Wireless client must know the SSID.
- Wireless client initiates the process of broadcasting a probe requesting frames on multiple channels.
- Hiding SSID is not secure to know existing access points.

## Beacon Frames

- Used by WLAN networks to advertise presence.
- Showing: SSID, security implementations, supported rates.

## AP Association Process

1. 802.11 probe.
2. Authentication.
3. Association.

## Wireless Intruders

- Someone searching for WiFi not meant to connect to the network.
- Due to weak security implementation.

**Interception of data** - Hackers that intercept are able to steal and listen to sensitive data.

**Rogue access point** - Capture data and servers gaining MAC address data.

## Denial of Attacks

- DOS improperly configured devices.
- Configuration errors on WLAN.
- Malicious user intentionally interfering with wireless communication.
- Accidental interference.
- 2.4 GHz is more prone to interference than 5 GHz bandwidth.
- WLANs operate in unlicensed frequencies but microwaves and cordless phones can cause interference.

## Management Frame DOS Attacks

- Spoofed disconnect attackers.
- Attackers use of disassociation commands create a burst of traffic.
- CTS flood: Attackers takes advantage of CSMA/CA method for bandwidth, repeatedly flood CTS.

## Rogue Access Points

- Connected to a network without authorization.
- Capture packets, data and MAC addresses or initiate man-in-the-middle attack (MITM).
- Monitor radio spectrum for unauthorized access point (AP).