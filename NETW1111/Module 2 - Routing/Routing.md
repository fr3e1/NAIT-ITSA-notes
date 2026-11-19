---
cssclasses: [netw1111]
course: NETW1111
module: "Module 2 - Routing"
lesson: "M2 Day 1 - Routing Concepts"
source: "Routing.pptx"
tags: [NETW1111, networking, routing, tracert]
---
# Routing
Back to [[NETW1111]] · Prev: [[TCP-IP and Transport Protocols]] · Next: [[Network Diagrams]]

## Why routers?
* Systems on the same LAN don't need a router
* To reach *other* networks (like the Internet) a router passes packets to a network "closer" to the destination
* A router has an **IP address in at least two networks**, and discovers other routers on each

## Router behaviour
* Routers also **block** some packets - **broadcasts do not cross routers**
	* e.g. ARP broadcasts, DHCP negotiations
* Too much broadcast traffic makes switches act like hubs → routers (and L3 switches) contain it
* Routers are a natural choke point so they make good **firewalls** - often combined, but a router doesn't *have* to be a firewall (see [[Firewalls]])

## Routing protocols
* Identify other routers and decide which to use for delivery; every router advertises itself

| Protocol | Transport | Details |
| --- | --- | --- |
| RIPv1 | UDP | Hop count - lowest hops wins. No longer used |
| RIPv2 | UDP | Adds CIDR support |
| OSPF | IP | Open Shortest Path First. Builds a topology map. Common in enterprise. IPv4/IPv6/CIDR |
| EIGRP | IP | Cisco proprietary alternative to RIP |
| BGP | TCP | Border Gateway Protocol. At the edge of an organization, talks to the wider Internet |
| IS-IS | Data link | OSI standard, used in large backbone provider networks |

* Routing is always considered **Network layer (L3)** regardless of transport
* **Routing tables** change over time as routes appear/fail - lets routers adapt to congestion and failure

## Routing clients and the default gateway
* Clients don't run routing protocols - they're **assigned a default gateway (DG)**
	* Dynamically via **DHCP**, or statically with the IP address
* `ipconfig /all` shows your DG
* With no other instructions, the client always sends off-LAN traffic to its DG

> [!IMPORTANT]
> **The DG must be on the same LAN**
> Clients reach the DG with **Ethernet / MAC addressing** (L2). So the client and DG must:
> 1. Share the **same network address**, **and**
> 2. Be reachable through the **same switch(es)**
>
> Same network but different switch = ❌. Same switch but different network = ❌. A very common cause of communication failures.

| Host IP | Switch (vmnet) | Default gateway | Reachable? |
| --- | --- | --- | --- |
| 192.168.202.25 | vmnet1 | 192.168.201.2 | ❌ different network, same switch |
| 192.168.202.25 | vmnet8 | 192.168.202.2 | ✅ |
| 192.168.201.80 | vmnet1 | 192.168.201.2 | ✅ |
| 192.168.201.80 | vmnet8 | 192.168.202.2 | ❌ different network, different switch |

## Static routes
* Override the default route for a specific destination network
* Less flexible than routing protocols - only use when needed
* Can be set on hosts or on routers

```powershell
route print                                          # view routing table (0.0.0.0 = default route)
route add 10.0.0.0 mask 255.0.0.0 192.168.202.3      # temporary static route
route -p add 10.0.0.0 mask 255.0.0.0 192.168.202.3   # -p = persistent (survives reboot)
route delete 10.0.0.0
route change ...
route /?                                             # help; -4 / -6 to force IPv4/IPv6
```

## Testing connections
* `ping` works across networks too, but firewalls may block it at the **sender**, **router** or **receiver**

### tracert / traceroute
* Uses ICMP with the **TTL** (time to live) field - a "best before date" measured in router hops
* Each router decrements TTL by 1; at 0 the router discards it and replies with an ICMP "TTL exceeded"
* tracert sends packets with TTL = 1, 2, 3... so each successive router reveals itself
* Reading output:
	* Col 1 = hop number (counts up from 1)
	* Cols 2-4 = round trip time for 3 probes
	* Last col = IP/name of the router at that hop
	* **First hop should be your default gateway** (or a static route)
	* `* * *` = a **black hole router** that returns no information

```powershell
tracert 8.8.8.8      # Windows
traceroute 8.8.8.8   # Linux
```

## Troubleshooting order
> [!TIP]
> **Can't reach a web page?**
> Possible failure points: **routing**, **DNS**, **web server**. Rule out the easy ones first:
> 1. Does **DNS** resolve? If not → DNS is most likely the problem
> 2. If DNS resolves, can you **ping/tracert the IP**? If not → routing issue (maybe outside your control depending on the hop)
> 3. If both pass → the problem is at the **server/application** level

## Summary
* Routers deliver packets between networks and need an interface + IP in each
* Clients use a default gateway (static or DHCP); static routes override it
* tracert/traceroute tests routing
* Routers often, but not always, act as firewalls

## Links
* [Routing protocol](https://en.wikipedia.org/wiki/Routing_protocol) · [OSPF](https://en.wikipedia.org/wiki/Open_Shortest_Path_First) · [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) · [Broadcast domain](https://en.wikipedia.org/wiki/Broadcast_domain)
* Videos: [Default Gateway Explained](https://www.youtube.com/watch?v=pCcJFdYNamc) · [Subnet Masks](https://www.youtube.com/watch?v=s_Ntt6eTn94) · [Traceroute](https://www.youtube.com/watch?v=up3bcBLZS74)
* Activities: *Building a Windows Router*, *Building a FreeBSD Router*
