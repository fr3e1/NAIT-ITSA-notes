---
course: SYSA1000
module: "Module 4 - Windows OS Configuration"
source: "SYSA1000 - Windows Config - Network Addressing and Name Resolution.pptx"
tags: [SYSA1000, windows, networking, ipv4, dns]
---
# Network Addressing and Name Resolution
Back to [[SYSA1000]] · Prev: [[Command Line, Firewall, and System Settings]] · Next: [[Windows Updates, Backup, and Restore]]

* The VM **network mode** decides what it can reach (Bridged / NAT / Host-only - see [[Virtual Machine Operations#Network modes]]). This module uses **NAT**

## Four values every address needs
| Value | Purpose |
| --- | --- |
| **IPv4 address** | Identifies this machine on its network |
| **Subnet mask** | Which addresses are on the same network |
| **Default gateway** | Where traffic for other networks goes |
| **DNS server** | Turns names into addresses |

## Dynamic vs static
* **Dynamic (DHCP)** - machine asks for an address; gets a **lease** (time limited) with mask, gateway and DNS. `ipconfig /all` shows the lease and which server issued it
* **Static** - typed in, never changes. For servers, printers, anything others find by address
	* Settings → Network & internet → Ethernet → IP assignment → **Edit** → **Manual** → IPv4 on → address, mask, gateway, preferred + alternate DNS

```powershell
New-NetIPAddress -InterfaceAlias Ethernet0 -IPAddress 192.168.202.50 -PrefixLength 24 -DefaultGateway 192.168.202.2
Set-DnsClientServerAddress -InterfaceAlias Ethernet0 -ServerAddresses 192.168.202.2
Set-NetIPInterface -InterfaceAlias Ethernet0 -Dhcp Enabled   # back to DHCP
```

## Testing connectivity in order
1. Ping the **default gateway**
2. Ping an **address on the internet** (e.g. `8.8.8.8`)
3. Ping a **name** (e.g. `dns.google`)
4. `nslookup` the name

Each success proves one more piece; the first failure shows where the path stops.

### Reading ping
| Result | Meaning |
| --- | --- |
| `Reply from ... time= TTL=` | Machine answered |
| `Request timed out` | Nothing back in 4 seconds |
| `Destination host unreachable` | No route to send it on |
> [!note] A blocked ping and a down machine look the same from outside

## Name resolution
* Windows checks its **cache** and **hosts file** first, then asks the **DNS server**
* Name fails but address works → **DNS problem**, not network
* `ipconfig /displaydns` and `ipconfig /flushdns`

### nslookup
* Asks the DNS server directly; first two lines show **which server** answered
* **Non-authoritative** = answer came from that server's cache
* Only asks the primary DNS server - can fail when ping (which uses other methods) works

## Breaking it on purpose
| Wrong value | Symptom |
| --- | --- |
| **Gateway** | Local network works, nothing beyond it |
| **Subnet mask** | Some local machines reachable, others not |
| **DNS server** | Every address works, no name does |

> [!tip] The symptom names the value - that's the skill

## The firewall is part of the path
* A ping that arrives still needs a firewall rule to be answered (see [[Command Line, Firewall, and System Settings#Why ping fails by default]])
* Test from **both directions** - they fail for different reasons
* A blocked ping is a finding, not proof the machine is down

Related: [[LANs and IP Addressing]], [[DHCP]], [[DNS]] (NETW1111)
