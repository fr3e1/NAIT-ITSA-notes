---
cssclasses: [netw1111]
course: NETW1111
module: "Module 3 - Application Layer Protocols"
lesson: "M3 Day 1 - Firewalls"
source: "Firewalls.pptx"
tags: [NETW1111, networking, firewalls, nat, security]
---
# Firewalls
Back to [[NETW1111]] · Prev: [[Subnetting Considerations]] · Next: [[Packet Sniffing]]

* A **firewall** enforces an **access control policy** between two or more networks
* A pair of mechanisms: **blocking** and **permitting** traffic
* Requires a good understanding of [[TCP-IP and Transport Protocols|TCP/IP]]
* Hardware or software based

## Firewall types / functions
### Network Address Translation (NAT)
* Internal **private** addresses communicate through one or more **external** addresses
* The NAT router rewrites internal source addresses to the external one, tracks the original, and rewrites replies back
* To reach an internal server from outside you need a **redirect (port forward)** rule
* With no redirect rule or established connection, inbound attempts are dropped → acts as an effective firewall

### Packet filtering
* Checks packet **headers** (source/destination IP and port) against rules
* Doesn't understand app protocols - only IPs and ports
	* e.g. SSH blocked on 22, but SSH set to listen on **80** gets through
* Modern ones are **stateful** - track connection state
* Very fast, transparent to users

### Proxy servers
* Break the client/server model - act as a **middleman**: client ↔ proxy, proxy ↔ server (two connections)
* Allows caching, user authentication, protocol-specific rules (e.g. allow inbound FTP, block outbound FTP)
* **Application specific** - each new protocol needs a proxy rule
* Examples: web proxies, DNS proxies

### Application gateways
* **Highest security**, at the cost of overhead/performance
* Intelligent middleman - packet contents verified, requests/acks interpreted
* No direct traffic between networks; elaborate auditing
* Needs lots of CPU/RAM and understanding of app protocols; less transparent
* More detailed audit reports and more conservative security than network layer firewalls

## Complex setups
* Combine routers and computers in multiple layers → **defense in depth**
* A failure at one layer is often caught by another
* **DMZ (demilitarized zone)** - a host or small subnet between the trusted internal LAN and the untrusted Internet; brokers requests for the LAN

## Limitations
Firewalls **can't** fully prevent:
* Viruses, worms, trojans
* Email attachment threats
* Phishing
* Application bugs on servers

Host firewalls can alert to unusual *outbound* activity, but only after the fact.

## Windows Firewall
* Rules apply separately to three **profiles**:

| Profile | When | Default inbound |
| --- | --- | --- |
| **Domain** | Connected to its AD domain | Some allowed (file/printer sharing) |
| **Private** | Connected to a private network (none by default) | Some allowed |
| **Public** | Default when no DC is reachable | **None** - great for public hotspots |

* Default: block everything not explicitly allowed **inbound**; allow all **outbound**
* Enabling a Windows feature auto-creates its firewall rules; third-party apps need rules created (often by the installer)
* Manage with **Windows Firewall with Advanced Security** console, **PowerShell**, or **Group Policy**
* Outbound filtering breaks core services (DHCP, DNS, Group Policy, IGMP) unless allowed - usually overkill
* **Scope** - restrict a rule to IPs, subnets, ranges or predefined computers (e.g. allow internal, block external)

### Group Policy path
```
Computer Configuration\Policies\Windows Settings\Security Settings\
  Windows Firewall with Advanced Security\Windows Firewall with Advanced Security
```

```powershell
Get-NetFirewallProfile
New-NetFirewallRule -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow
```

## Links
* Video: [What is a Firewall?](https://www.youtube.com/watch?v=kDEX1HXybrU)
* Activities: *Exercise - Firewalls*, *Using NAT with a pfSense Firewall Router* - see [[Firewall Lab]]
