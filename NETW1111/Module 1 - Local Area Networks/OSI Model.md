---
course: NETW1111
module: "Module 1 - Local Area Networks"
lesson: "M1 Day 2 - OSI Model"
source: "OSI Model.pptx"
tags: [NETW1111, networking, osi]
---
# OSI Model
Back to [[NETW1111]] · Prev: [[Intro to Networking]] · Next: [[LANs and IP Addressing]]

## What is OSI?
* **Open Systems Interconnect** model, created by the **ISO** (International Standards Organization) in the 1970s
* Started as a *model* of networking that divides it into separate but interdependent functions called **layers**
* Each layer passes data up or down the stack to the layers above/below
* Has stood the test of time for clarity and ease of understanding

## ARPANET
* Meanwhile the US Department of Defense incubated the Internet under **DARPA**
* ARPAnet (predecessor to the Internet) used its own **TCP/IP** suite, not OSI
* Real Internet infrastructure was built independently of OSI, but OSI is still the best model to *understand* networking

## The seven layers
> [!tip] Mnemonic (top → bottom)
> **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing

| # | Layer | Concerned with | Examples |
| --- | --- | --- | --- |
| 7 | Application | Protocols used by network programs | HTTP, FTP, email, Windows file shares, APIs |
| 6 | Presentation | Structuring data for applications | Formatting, encoding, compression, encryption |
| 5 | Session | Managing connections | Establish/teardown, logon/logoff, pausing streams |
| 4 | Transport | Quality of service, rate of transfer | Fragmentation/reassembly, retransmission, flow rate (TCP/UDP) |
| 3 | Network | Delivery *between* networks | Routers, IP addressing, routing tables |
| 2 | Data Link | Framing data for the LAN | MAC addresses, switches, Ethernet |
| 1 | Physical | How signals are transmitted | Cables, radio waves, NICs, hubs |

### Layer 1 - Physical
* How signals are transmitted, repeated and boosted
* Media (cables, radio waves), hardware (NICs, cables, hubs, switches)
* Standards for signal timing, electrical interference, error detection

### Layer 2 - Data Link
* Structures data from the Network layer into **frames** for the Physical layer
* LANs transmit data at this layer
* Analogy: if Physical = roads and cars, Data Link = the *rules of the road*
* Two sublayers:
	* **MAC** (Media Access Control)
		* Every NIC has a unique MAC address burned into firmware - like a licence plate
		* First half = **vendor ID**, second half = **device ID** (like area code + number)
	* **LLC** (Logical Link Control)
		* Follows the rules of transmission on the LAN: packaging frames, optional sync and error detection
		* Supports many transmission protocols at once (ethernet, wireless, cell, powerline)

### Layer 3 - Network
* Delivery of data between *different* networks
* Routers and IP addressing live here
* Determines delivery paths (routing tables) and recalculates paths when failures happen (avoiding traffic jams)

### Layer 4 - Transport
* Quality of service and rate of transfer between client and server
* Detects errors and retransmits when needed
* Fragmentation, reassembly, rate of data flow

### Layer 5 - Session
* Often implemented by application developers
* Connection establishment/teardown, user logon/logoff, pausing connections (movie/audio streams)

### Layer 6 - Presentation
* Also often implemented by app developers
* Data formatting, encoding, compression/decompression, encryption/decryption

### Layer 7 - Application
* Protocols used by programs we know (browsers, mail clients)
* Not every app is part of this layer - only ones that use network protocols

## Example: web client/server
| Web client (send) | Web client (receive) | Layer | Web server (send) | Web server (receive) |
| --- | --- | --- | --- | --- |
| Type URL | View page | Application | Supply web content | Process URL request |
| Parse URL request | Display HTML | Presentation | HTML, ASCII, encoding | Interpret HTTP URL request |
| Negotiate connection | Maintain connection | Session | Maintain connection | Negotiate connection |
| Create data packets | Extract data from packets | Transport | Create data packets | Extract data from packets |
| Determine delivery path | Extract data from packets | Network | Determine delivery path | Extract data from packets |
| LAN transmission | LAN transmission | Data Link | LAN transmission | LAN transmission |
| Send signals | Receive signals | Physical | Send signals | Receive signals |

> [!note] Keep in mind
> * **All** layers are used at any given time by every networked application
> * But each layer doesn't need to understand the others - a browser calls the OS, the OS talks to the NIC
> * This separation lets new tech be inserted into one layer without disturbing the others

## Links
* [OSI model](https://en.wikipedia.org/wiki/OSI_model) · [ARPANET](https://en.wikipedia.org/wiki/ARPANET) · [Logical link control](https://en.wikipedia.org/wiki/Logical_link_control)
* Video: [What is OSI Model](https://www.youtube.com/watch?v=0y6FtKsg6J4)
* Activity: *Name That Layer* worksheet
