---
cssclasses: [netw1111]
course: NETW1111
module: "Module 2 - Routing"
lesson: "M2 Day 3 - Subnets 1"
source: "Subnetting.pptx"
tags: [NETW1111, networking, subnetting, cidr]
---
# Subnetting
Back to [[NETW1111]] · Prev: [[Network Diagrams]] · Next: [[Subnetting Considerations]]

## Why CIDR?
* **Classful** addressing (A/B/C in 8-bit octets) didn't scale - few orgs need a class A, so huge numbers of addresses were wasted
* **CIDR** (Classless Inter-Domain Routing) was created in **1993** to slow IPv4 exhaustion and shrink internet routing tables
* Classful network:host ratios: A = 8:24, B = 16:16, C = 24:8
* CIDR "**steals**" bits from the host portion to create more networks → **subnets**

## Network and broadcast addresses
* In `192.168.202.0/24`:
	* Host part all 0s → `192.168.202.0` = the **network ID**
	* Host part all 1s → `192.168.202.255` = the **broadcast address**
	* Those two are reserved → **254 usable hosts** (1-254)

## Stealing bits
* Steal the **most significant (leftmost)** host bits for the subnet; the rest stay as host IDs
* Example - steal 1 bit from a class C:

| Subnet | Subnet ID | Usable hosts | Broadcast |
| --- | --- | --- | --- |
| `192.168.202.0/25` | .0 | .1 - .126 | .127 |
| `192.168.202.128/25` | .128 | .129 - .254 | .255 |

* Mask `/25` = `255.255.255.128`
* 7 host bits → 2^7 = 128 addresses, minus 2 = **126 usable**

> [!TIP]
> **Reading a CIDR mask**
> Compare against the class default: class C expects /24, mask is /25 → 25 − 24 = **1 bit stolen** → 2^1 = 2 subnets

## Formulas
* **Subnets** = 2^(stolen bits)
* **Usable hosts per subnet** = 2^(host bits) − 2
* Bits must be stolen whole - subnets always **double** (2, 4, 8, 16...)

| CIDR | Mask | Stolen bits (class C) | Subnets | Usable hosts | Block size |
| --- | --- | --- | --- | --- | --- |
| /24 | 255.255.255.0 | 0 | 1 | 254 | 256 |
| /25 | 255.255.255.128 | 1 | 2 | 126 | 128 |
| /26 | 255.255.255.192 | 2 | 4 | 62 | 64 |
| /27 | 255.255.255.224 | 3 | 8 | 30 | 32 |
| /28 | 255.255.255.240 | 4 | 16 | 14 | 16 |
| /29 | 255.255.255.248 | 5 | 32 | 6 | 8 |
| /30 | 255.255.255.252 | 6 | 64 | 2 | 4 |
| /31 | 255.255.255.254 | 7 | 128 | 0 | 2 |

## Routing implications
* Hosts in `.0/25` **can't reach** hosts in `.128/25` without a **router**
* **Broadcasts are blocked** between subnets - reduces broadcast traffic, but affects broadcast-based protocols like [[DHCP]]
* You're trading fewer hosts per network for more networks

## The pizza analogy 🍕
* Host bits = a pizza to share equally
* 1 bit = 2 slices, 2 bits = 4 slices, 3 bits = 8 slices (/27)... always doubling - "there are no half-bits"
* Eventually the slices are too small to feed anyone (/31 → no usable hosts)

> [!WARNING]
> **Plan for growth**
> Too many small subnets means restructuring later when more devices arrive. Better to waste a few addresses than redo the network.

## Larger networks
* Subnetting is most effective on big address spaces - ISPs own large public ranges and subnet them for customers
* Private `10.0.0.0` and `172.16.0.0` - `172.31.0.0` can be subnetted for internal segmentation, e.g. `10.0.0.0/16` behaves like a class B
* **Supernetting** - the reverse: steal *network* bits to get more hosts (e.g. a supernet of `192.168.0.0/16`). Allows mixed sizes (/25 and /26)

## IPv6
* 48 bits global routing prefix + **16 bits subnet** + 64 bits host - so many subnets you'll rarely need more

## Links
* [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) · [Subnet](https://en.wikipedia.org/wiki/Subnet) · [IP subnet calculator](https://www.calculator.net/ip-subnet-calculator.html)
* [Barto's subnetting lecture (2020)](https://www.dropbox.com/scl/fi/p3qd3l884fh5p8ikgxrm5/Subnetting-video-lecture-for-DMIT1518-by-BARTO.mp4?rlkey=j95ivsr7wzwzlic47jznrv4yh&dl=0)
* Videos: [Subnets vs VLANs](https://www.youtube.com/watch?v=6_giEv20En0) · [Layer 2 vs Layer 3 Switches](https://www.youtube.com/watch?v=bdNS0K4Bt8U) · [What is a Guest Network?](https://www.youtube.com/watch?v=R1qJ87Fo5_k)
* Activities: *Exercise - Subnetting*, *RQ*, `subnetting-static-routing.pkt` (Packet Tracer)
