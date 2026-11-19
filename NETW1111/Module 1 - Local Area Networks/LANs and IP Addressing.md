---
cssclasses: [netw1111]
course: NETW1111
module: "Module 1 - Local Area Networks"
lesson: "M1 Day 3 - IP Addressing"
source: "LANS.pptx"
tags: [NETW1111, networking, lan, ipv4, ipv6]
---
# LANs and IP Addressing
Back to [[NETW1111]] · Prev: [[OSI Model]] · Next: [[Ethernet, ARP and ICMP]]

## Local area networks
* Simplest form of networked computing
* A LAN is any set of computers that **share an address space** and are connected by a common transmission medium
* Modern wired networks use a **star topology**
* No filtering of data - nodes essentially have direct access to each other

## Broadcast domains
* A LAN = the portion of a network where **broadcast traffic is not restricted**
* A broadcast is transmitted to *all* nodes on the network (they may ignore it, but still physically receive it)
* Switches and hubs pass on broadcasts, so they can be chained to extend a LAN

## Hubs vs switches
| | Hub | Switch |
| --- | --- | --- |
| Forwarding | Sends **all** data down **all** ports | Learns which node is on which port; unicasts only go to that port |
| Security | Anyone on the hub can snoop all traffic | Much better |
| Collisions | All ports in one **collision domain** | Each port is its own collision domain |
| Bandwidth | Shared | Per port (1 Gbps switch = 1 Gbps per node) |
| Today | Rarely seen | Standard |

* Broadcasts still go down all ports on a switch

## Addressing
* Each node needs a unique identifier (**address**) to receive data
* Two types: **IPv4** (first, still widely used) and **IPv6** (created to "address" IPv4's issues)

### IPv4 address example
```
32-bit binary:      11011000011111100001111110101010
Octets (8 bits):    11011000 01111110 00011111 10101010
Dotted decimal:     216.126.31.170
```

### Network and host
* Leading *n* bits = **network ID**, remaining bits = **host ID**
* Analogy: surname (network/family) + given name (host, unique within the family)
* More network bits → fewer possible hosts per network
* Shown with **CIDR notation** or a **subnet mask**:
	* `192.168.202.4/24` → the first 24 bits are the network
	* same as `192.168.202.4` mask `255.255.255.0`

### Network classes
| Class | Leading bits | First octet | # Networks | # Hosts | Mask / CIDR |
| --- | --- | --- | --- | --- | --- |
| A | 0 | 1-126 | 126 | 16,777,214 | 255.0.0.0 /8 |
| B | 10 | 128-191 | 16,382 | 65,534 | 255.255.0.0 /16 |
| C | 110 | 192-223 | 2,097,150 | 254 | 255.255.255.0 /24 |
| D | 1110 | 224-239 | multicast | - | - |
| E | 1111 | 240-254 | experimental | - | - |

### Private addresses
| Class | Private networks | Mask | Range |
| --- | --- | --- | --- |
| A | 10.0.0.0 (127.0.0.0 reserved for loopback) | 255.0.0.0 | 10.0.0.0 - 10.255.255.255 |
| B | 172.16.0.0 - 172.31.0.0 | 255.255.0.0 | 172.16.0.0 - 172.31.255.255 |
| C | 192.168.0.0 - 192.168.255.0 | 255.255.255.0 | 192.168.0.0 - 192.168.255.255 |

## IPv6
* IPv4's ~4 billion addresses weren't enough
* **Dual stacking** = a network supports both IPv4 and IPv6
* 128-bit addresses:

| Global routing prefix | Subnet ID | Interface ID |
| --- | --- | --- |
| 48 bits | 16 bits | 64 bits |

* Prefix length = 48 + 16 = **/64**

### Special IPv6 addresses
* **Link local** - not routable, LAN only. Every IPv6 interface gives itself one
* **Unique local** - routable but not on the internet (like IPv4 private). Manual or DHCP
* **Global** - routable on the internet. Purchased or leased

### IPv6 notation
```
Full:                       2001:0db8:0000:0000:0000:0000:1518:0001
Drop leading 0s per hextet: 2001:db8:0:0:0:0:1518:1
Longest run of 0s -> "::":  2001:db8::1518:1
```
> [!WARNING]
> `::` can only be used **once** in an address

### Common IPv6 addresses
| Address | Meaning |
| --- | --- |
| `::/0` | All routes (= `0.0.0.0` in IPv4), used in static routes |
| `::/128` | Temporary address until a link local is assigned |
| `::1/128` | Loopback (= `127.0.0.1`) |
| `FE80::/10` | Link-local unicast (like APIPA `169.254.x.x`) |
| `FF00::/8` | Multicast |
| everything else | Global unicast (internet routable) |

## Transmission types
* **Unicast** - node to node
* **Broadcast** - IPv4 only; all nodes receive it. Too much slows the network
* **Multicast** - only nodes that advertise interest receive it; switches only send down participating ports
* **Anycast** - IPv6 only; delivered to *one* of several potential targets

## Links
* Videos: [IP Addresses](https://www.youtube.com/watch?v=ThdO9beHhpA) · [The Ping Command](https://www.youtube.com/watch?v=IIicPE38O-s) · [Public vs Private IP Addresses](https://www.youtube.com/watch?v=po8ZFG0Xc4Q)
* Activity: *Exercise - Testing Connectivity*
