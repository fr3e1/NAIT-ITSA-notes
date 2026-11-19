---
cssclasses: [netw1111]
course: NETW1111
module: "Module 1 - Local Area Networks"
lesson: "M1 Day 4 - Ethernet"
source: "Ethernet-ARP-ICMP.pptx"
tags: [NETW1111, networking, ethernet, arp, icmp]
---
# Ethernet, ARP and ICMP
Back to [[NETW1111]] · Prev: [[LANs and IP Addressing]] · Next: [[TCP-IP and Transport Protocols]]

## Ethernet
* Standard for transmitting data across switches and hubs in a LAN
* Lives at the **Data Link layer** (L2) of the [[OSI Model]]
* Data is broken into **frames** / protocol data units (PDUs) - **Ethernet II frames**
* Uses **MAC addresses** (6 bytes, burned onto each NIC) to identify hosts

### Collision domains and CSMA/CD
* **Collision domain** = area where transmissions can disrupt each other
* Classic Ethernet used **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection)
	* Like a crowd all talking at once: if two talk at the same time, both stop, wait a random time, and retry
	* "A giant transmission food-fight free-for-all"
* Switches and routers segregate collision domains → CSMA/CD unnecessary today, but useful for understanding vulnerabilities

### Ethernet II frame header
| Field | Purpose |
| --- | --- |
| Preamble | 56 alternating 1/0 bits for signal timing |
| Start of frame delimiter | Marks end of preamble, start of real data |
| Destination MAC | Intended host (or the broadcast address) |
| Source MAC | Sending host |
| Type | Payload type (IPv4, ARP...) or VLAN number |
| Data | Payload, padded to minimum size |
| Frame check sequence | CRC error checking |

## ARP - Address Resolution Protocol
* On a LAN, hosts transmit using **MAC** addresses, but they usually only know the **IP** (from DNS)
* ARP finds the MAC address for a known IP:
	1. Client **broadcasts** "who has IP x.x.x.x?"
	2. That host replies directly to the requester with its IP + MAC
	3. Requester stores it in the **ARP cache** (time varies by OS) → fewer broadcasts
* ARP is **not routable** - LAN only
	* Traffic leaving the LAN is sent to the **default gateway's** MAC (found with ARP)
	* The router then uses its own MAC on the next network

> [!WARNING]
> **ARP spoofing**
> ARP has no built-in security. A malicious host can falsely advertise itself as another host (detectable, though).

```
arp -a        # view the ARP cache (Windows/Linux)
```

## ICMP - Internet Control Message Protocol
* Helper protocol for TCP/IP: connectivity testing and diagnostics
* Common messages:
	* **Destination unreachable** - host or network can't be reached
	* **Redirect** - use a different route (only routers use this)
	* **Echo request/reply** - basic connectivity testing (ping)
	* **Time to Live exceeded** - packet traversed too many networks, discarded
	* **Flow control** - fragmentation requirements
* Header: **Type** (category), **Code** (subtype), **Checksum**, **Rest of header** (4 bytes, varies)

### Ping
* **P**acket **In**ter**N**et **G**roper - tests basic connectivity using echo request/reply
* Firewalls may block echo requests (prevents network mapping)
* Routers that don't respond to ping = **black hole routers**

## Links
* [Ethernet](https://en.wikipedia.org/wiki/Ethernet) · [CSMA/CD](https://en.wikipedia.org/wiki/Carrier-sense_multiple_access_with_collision_detection) · [ARP spoofing](https://en.wikipedia.org/wiki/ARP_spoofing) · [ICMP](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol) · [Ping of death](https://en.wikipedia.org/wiki/Ping_of_death)
* Videos: [Collision vs Broadcast Domain](https://www.youtube.com/watch?v=C2FrTZxi_NI) · [ARP Explained](https://www.youtube.com/watch?v=cn8Zxh9bPio) · [Half vs Full Duplex](https://www.youtube.com/watch?v=LMRSS7ZYM50)
* Activity: *Exercise - MAC Addresses*
