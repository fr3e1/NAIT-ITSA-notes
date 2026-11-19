---
cssclasses: [netw1111]
course: NETW1111
module: "Module 2 - Routing"
lesson: "M2 Day 2 - Network Diagrams"
source: "Network Diagrams.pptx"
tags: [NETW1111, networking, diagrams, documentation]
---
# Network Diagrams
Back to [[NETW1111]] · Prev: [[Routing]] · Next: [[Subnetting]]

* A visual representation of the devices on a network - used as **documentation** and a **troubleshooting tool**
* You should be able to **recreate your network** from the diagram + documentation

## Physical vs logical
* **Physical** - the layout of cables connecting devices
* **Logical** - how devices transmit data (networks, IPs, routing)

## What goes in a diagram
* **Title block** - title, your name, date, version
* **Legend** - every stencil used
* Proper icon for each device + device info (hostname, IP, model)
* Port assignments
* IP addressing info (consider a separate IP table)

## Common icons (Cisco style)
| Device | Icon | Notes |
| --- | --- | --- |
| Router | Circular disc, 4 arrows pointing out | Connects 2+ distinct networks |
| Layer 2 switch | Box with two pairs of opposite arrows | Data link only |
| Layer 3 switch | Switch with routing symbol | L2 + L3; routes between **VLANs** without a separate router |
| PC / host | Classic tower | Monitors/laptops also used |
| Printer | Printer icon | |
| Wireless access point | | Gives wireless devices access to the *wired* network (enterprise) |
| Wireless router | | Own wireless network + address space, routes to wired (home/SMB) |
| Firewall | Brick wall | Firewall router = router icon with bricks |
| Server | Generic box | Specific icons for web server etc. |
| Mainframe | Black IBM icon | |
| Database | Plain cylinder | |
| VoIP phone | Phone | Usually on its own VLAN for sound quality |
| Hub | | Rare; sometimes used for packet capture since it repeats everything |
| Cloud / Internet | Cloud | Unknown external networks - "there be dragons" |

## Colour coding (awareness only)
* Black = default · Blue = unknown · Green = normal · Red = critical · Gold = unmanaged · Grey = unreachable

## Tips for clean diagrams (Visio)
* Label figures and reference them in your writing ("See Figure 1")
* Align text and shapes (alignment tools, **F8**)
* **Avoid directional arrows** - they imply one-way traffic
* Don't overlap text with shapes; hide connector ends behind shapes
* Group related shapes with **Ctrl+G**
* Consistent symbols for similar devices
* Keep it simple - split large networks into multiple diagrams
* Only include IPs / VLAN IDs if they add value
* Use layers or lock background elements

## Links
* [IT Glue - network diagrams](https://www.itglue.com/blog/network-diagrams/)
* [Create a basic network diagram (Visio)](https://support.microsoft.com/en-us/office/create-a-basic-network-diagram-f2020ce6-c20f-4342-84f7-bf4e7488843a)
* [Cisco network topology icons](https://www.cisco.com/c/en/us/about/brand-center/network-topology-icons.html)
