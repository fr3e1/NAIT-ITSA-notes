---
cssclasses: [netw1111]
course: NETW1111
module: "Module 1 - Local Area Networks"
lesson: "M1 Day 1 - Intro to Networking"
source: "Networking.pptx"
tags: [NETW1111, networking, lan, wan]
---
# Intro to Networking
Back to [[NETW1111]]

* The idea behind networking is that computers are more useful in communication with each other than they are in isolation
* Networking introduces new problems to solve, but also new opportunities
* The internet is the largest example of both the benefits and drawbacks of networking

## Opportunities
* When we network computers, they can work together to solve more complex or larger scale problems. Some examples are:
	* Digital render farms
	* Cloud technology (clusters of servers providing storage, email, streaming, etc.)
	* Online gaming, social media
	* Distance education or remote work

## Drawbacks
* Networking systems together introduces both computers and users to new problems as well. Some issues that have had to be addressed are:
	* The mechanics of how systems interact with each other (physical media)
	* Security effects of connecting systems together (firewalls, anti-virus, remote exploits)
	* How to connect different devices together (protocol standards)
	* Troubleshooting the interaction between networks and applications

## LANs
* The smallest network is a **LAN** (local area network), found in homes, businesses and schools
* Generally confined to a single floor, building, or group of buildings
* A large enterprise may have multiple LANs connected together
* LANs can come in different topologies, including bus and ring topologies, but the **star topology** has dominated in recent decades
	* Nodes pass through an intermediate device (switch) to talk to each other
	* That device is a single point of failure, but the benefits won out over the risks

## WANs
* When networking extends further than the confines of a LAN, we call it a **WAN** or wide area network
* Municipalities, provinces, states and countries form WANs
* WAN tech differs from LAN tech because it must:
	* handle much larger amounts of data at once
	* survive transmission over much larger distances
* The **Internet** is the largest WAN in history, a patchwork of many technologies

## Physical media
| Technology | Medium | Application |
| --- | --- | --- |
| CAT5/5E/6/7/8 | Ethernet cable (copper) | LAN |
| Fiber optic | Glass | ISP/WAN |
| Wireless | Radio waves | WAN/LAN |
| Satellite | Radio waves | Rural/isolated |
| Powerline | Electric cable (copper) | Residential |
| Dialup/ADSL *(being phased out)* | Telephone line (copper) | ISP to client |
| MoCA/Cable *(being phased out)* | Coaxial cable (copper) | ISP to client |

* Most LANs use some combination of copper, wireless and fiber
* Cable/ADSL ISP connections are being superseded by fiber

## Security
* Networking increases each node's vulnerability since many more hostile entities can reach it
* Risks: targeted hacking, malware/viruses, phishing/spam, denial of service, eavesdropping/snooping
* Responses (⭐ = covered in this course): firewall routers ⭐, host based firewalls ⭐, anti-virus, anti-spam, encryption ⭐

## Protocol standards
* Nodes don't need to run the same OS, but they need to support a common set of **protocols**
* A protocol is just an agreed upon method to accomplish a task (here: communication)
* The Internet settled on the **TCP/IP suite**: TCP (Transmission Control Protocol) + IP (Internet Protocol)
* Other protocols in the suite covered in this course: UDP, ARP, HTTP, FTP, DHCP, DNS, TLS/SSL, ICMP, SMB, RDP, SSH

## Troubleshooting
* Networks aren't as simple or as fast as advertised; when the network is down, sysadmins get involved
* Common causes of network problems:
	* Disconnected or damaged cables
	* Poor wireless signal strength
	* Hardware issues (switch, NIC, router)
	* Misconfigured OS network settings, switch settings or router settings
	* Wrong or bad drivers
	* Mismatched protocol support between nodes
	* Misconfigured firewalls or services
* Sometimes the "problem" is the intended design (e.g. a firewall deliberately blocks the service, or the user is violating corporate policy)
* Knowing the source of a problem tells you what you can fix and what needs to be routed "higher"

> [!TIP]
> **Course goal**
> Separate problems into **hardware vs software**, **client vs server**, **network vs node**, and **user vs system** (PEBKAC: Problem Exists Between Keyboard And Chair). It's a binary process of ruling out and narrowing down causes.

## Links
* [Local area network](https://en.wikipedia.org/wiki/Local_area_network)
* [Wide area network](https://en.wikipedia.org/wiki/Wide_area_network)
* [Network topology](https://en.wikipedia.org/wiki/Network_topology)
* Video: [Virtualization Explained](https://www.youtube.com/watch?v=UBVVq-xz5i0)

Next: [[OSI Model]]
