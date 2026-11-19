---
cssclasses: [netw1111]
course: NETW1111
module: "Module 3 - Application Layer Protocols"
lesson: "M3 Day 5 - DNS Protocol"
source: "DNS.pptx"
tags: [NETW1111, networking, dns, name-resolution]
---
# DNS
Back to [[NETW1111]] · Prev: [[FTP]] · Next: [[DHCP]]

* **Domain Name System** - maps **host names to IP addresses** (IPv4 and/or IPv6)
* Hierarchy of DNS servers worldwide; every client has a **primary DNS server**
* **Port 53**, mainly **UDP** (TCP also used, e.g. zone transfers)

## Other name resolution (LAN only)
* **LLMNR** (Link-Local Multicast Name Resolution, aka *network discovery*)
	* Multicast, IPv4 + IPv6, Vista/Server 2008+
	* Not compatible with older products; a host without it enabled won't answer
* **NetBIOS** - legacy, back to DOS
	* Methods: LAN **broadcasts**, **WINS** (directory on an elected master server), **LMHOSTS** (static local file)
	* Long history of security bugs - not recommended in high security environments
	* No IPv6 support; IPv6 uses SMB over TCP instead
* Windows order: **DNS → LLMNR → NetBIOS**

## DNS hierarchy
* Tree starting at **13 root servers** - authoritative for the **TLDs**
* **ICANN** coordinates domain names, IP address assignment, and standard port numbers; you rent domains through **registries/registrars**

### Terminology
| Term | Meaning |
| --- | --- |
| TLD | Top Level Domain (`.com`, `.edu`, `.org`, `.gov`, `.net`, `.ca`) |
| Root server | One of 13 servers responsible for the TLDs |
| Resource record (RR) | Data returned from a DNS server |
| Domain name | Hierarchical name separated by periods (`nait.ab.ca`) |
| Zone | Area of DNS one server is responsible for (one server can host many zones) |

## Queries
* If the server hosts the zone → **authoritative answer**
* Otherwise it finds the responsible server:
	* **Iterative** - returns the *next server to ask*
	* **Recursive** - server tracks down the *final answer* for the client
* The **client** chooses which type (clients usually ask recursive)

## Resource records
| Type | Purpose |
| --- | --- |
| **SOA** | Start of authority - authoritative server + admin info (TTL, zone transfer interval, serial) |
| **A** | IPv4 address of a host |
| **AAAA** | IPv6 address of a host |
| **NS** | Name servers for a zone |
| **CNAME** | Alias for an A/AAAA record |
| **MX** | Mail (SMTP) servers for the zone |
| **PTR** | Reverse lookup - IP → name |
| **SRV** | A service available on the network (used heavily by AD) |

## Windows client resolution order
1. **hosts file** - `C:\Windows\System32\drivers\etc\hosts` - overrides everything
2. **DNS cache** - previously resolved queries (entries expire after 1 day by default)
3. **Primary DNS server** (recursive query) → upstream servers until authoritative
4. **Alternate DNS servers** - only after a *timeout* (not after a negative answer - any response stops the search)
5. Fall back to NetBIOS / LLMNR

```powershell
ipconfig /displaydns   # view cache
ipconfig /flushdns     # clear cache
```

## Server software
* **Microsoft DNS** - proprietary extensions, integrated with **Active Directory**
* **BIND** (Berkeley Internet Name Domain) - Linux/Unix, runs most of the public web; most root servers
* **NSD** - open source; 4 root servers. No root server uses Microsoft DNS

### Server roles
* **Primary** - authoritative for its zones
* **Secondary** - answers queries, syncs from the primary periodically (based on SOA timers)
* **Caching** - not responsible for any zones; caches answers to cut outgoing queries

## Firewalls and security
* Allow TCP/UDP **53 in**, high ports out
* ⚠️ Misconfigured servers allowing **full zone transfers** let attackers map internal networks
* A hacked DNS server can redirect critical services to malicious hosts

## nslookup
* Queries a DNS server **directly** - ignores the hosts file and cache, so it can detect those overrides
* Can query a specific server other than your primary

```powershell
nslookup www.google.ca                 # non-interactive
nslookup www.google.ca 8.8.8.8         # ask a specific server

nslookup                               # interactive
> set type=MX                          # A, AAAA, CNAME, MX, PTR, NS, SOA, SRV
> google.ca
> ls -d ad.example.ca                  # attempt a zone transfer
> exit
```

## Links
* Videos: [How a DNS Server works](https://www.youtube.com/watch?v=mpQZVYPuDGU) · [DNS Records Explained](https://www.youtube.com/watch?v=HnUDtycXSNE)
* Activities: *Exercise - DNS*, *DNS Troubleshooting*
* See also: [[DNS Server Role]] (SYSA1010)
