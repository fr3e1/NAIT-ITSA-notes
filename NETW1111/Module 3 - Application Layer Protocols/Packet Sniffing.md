---
cssclasses: [netw1111]
course: NETW1111
module: "Module 3 - Application Layer Protocols"
lesson: "M3 Day 2 - Packet Sniffing and Scanning"
source: "Packet-Sniffing.pptx"
tags: [NETW1111, networking, wireshark, nmap, security]
---
# Packet Sniffing
Back to [[NETW1111]] · Prev: [[Firewalls]] · Next: [[HTTP]]

* **Protocol analysis** / packet sniffing = examining network communications by capturing packets
* Tool = **protocol analyzer** (sniffer) - captures everything through the NIC and lets you search/filter it

## Uses
* Troubleshooting misbehaving applications
* Finding malicious behaviour
* Reverse engineering unpublished protocols
* Hacking
* Testing firewalls, switches, routers
* Study

## Passive vs active
* **Passive** - listen to real-time traffic. Hard to detect
* **Active** - send packets and study the responses (e.g. firewall testing, **nmap**). Easier to detect
* Both can gather stats: utilization, broadcast/unicast ratio, protocol use, packets/sec

## Wireshark
* Open source, Linux/Mac/Windows, GUI with filters (otherwise it's a needle in a haystack)
* NIC must be in **promiscuous mode** to capture frames for other MACs
	* Most wired NICs support it, many wireless NICs don't
	* Without it you still see broadcasts and traffic for your host
* **Filters** (during or after capture): broadcasts, ports, src/dst IP, src/dst MAC, multicast
* **Colour coding** of packet types (customizable)
* Captures are buffered in RAM and can be saved as **PCAP** files (wireshark.org has samples)

```text
# Handy Wireshark display filters
ip.addr == 192.168.202.10
tcp.port == 80
arp
dns
http.request
eth.dst == ff:ff:ff:ff:ff:ff
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

## Switches make sniffing harder
* Hubs sent everything everywhere; switches only send to the relevant port
* **Network tap** (legit) - IN, OUT and **Monitor** port; a sniffer or IDS listens on Monitor
* **MAC flooding** (dubious) - spam the switch with random MACs to overflow its MAC table → it falls back to sending everything down all ports (acts like a hub)
	* Defence: auto-shutdown ports that present more than a threshold number of MACs (port security)

## Links
* *Wireshark User Guide* (PDF in Brightspace)
* Video: [Data Packets - How does the internet send data?](https://www.youtube.com/watch?v=KjD3KANH-xc)
* Activities: *Exercise - Packet Sniffing*, *Exercise - Nmap Scanning*
