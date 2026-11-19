---
cssclasses: [sysa1010]
course: SYSA1010
module: "Module 1 - Windows Server Deployment"
source: "Lesson 03a - DNS Server Role.pptx"
tags: [SYSA1010, windows-server, dns]
---
# DNS Server Role
Back to [[SYSA1010]] · Prev: [[DHCP Server Role]] · Next: [[File Permissions and Network Shares]]

> [!TIP]
> Protocol details (record types, query types, client behaviour) are in [[DNS]] (NETW1111)

## Domain names
* Human-readable address converted to an IP by a DNS server; owned by a person or organization
* Hierarchy, separated by `.` and read **right to left**:

```
nait-ca . mail . protection . outlook . com . (root)
hostname  subdomain  subdomain   2nd-level TLD  implied
```

## Authoritative vs non-authoritative
* **Authoritative** - "owns" the domain; records are created and managed there. The DNS service on an AD domain controller owns the AD domain
* **Non-authoritative** - answers on behalf of the authoritative server (iterative/recursive), may **cache** responses

## Zones
| Zone | Maps | Records |
| --- | --- | --- |
| **Forward lookup** | Name → IP | A, AAAA, MX, TXT, CNAME, SRV, NS... |
| **Reverse lookup** | IP → name | **PTR**. Named by the classful subnet **reversed** + `.in-addr.arpa` (e.g. `192.168.1.0/24` → `1.168.192.in-addr.arpa`) |

* Zone types: **Primary**, **Secondary**, **Stub**
* **Active Directory-integrated** - zone stored in AD; only on DCs that also run DNS (replicates with AD, allows secure dynamic updates)

| Record | Purpose |
| --- | --- |
| A | Name → IPv4 |
| AAAA | Name → IPv6 |
| MX | Mail servers for the domain |
| TXT | Arbitrary text (SPF, verification) |
| CNAME | Alias pointing at another record |
| PTR | IP → name (reverse zone) |

## Dynamic update
* Records updated automatically, used with **DHCP**
* Done by the **DNS client** on the endpoint, or by the **DHCP server** if the client can't/doesn't
* Creates the record if missing; updates an existing A/AAAA **unless it's static**

## Forwarders
* A DNS server only knows its own zones. To resolve external names:
	* **Global forwarders** - send everything else to specific upstream servers
	* **Conditional forwarders** - for a specific domain, forward to specific servers
	* **Root hints** - query the **13 root servers (A-M)**, run by 12 organizations. Slow → best practice is to **disable root hints** and use forwarders

## Testing
```powershell
nslookup -type=MX nait.ca 192.168.1.2
Resolve-DnsName -Name nait.ca -Server 192.168.1.2 -Type MX
Add-DnsServerPrimaryZone -Name "example.local" -ReplicationScope Domain
Add-DnsServerPrimaryZone -NetworkId "192.168.1.0/24" -ReplicationScope Domain
Add-DnsServerResourceRecordA -ZoneName "example.local" -Name "web" -IPv4Address 192.168.1.20 -CreatePtr
Add-DnsServerForwarder -IPAddress 8.8.8.8
```

## Troubleshooting
* **Lookups failing:** right record type with right data? DHCP configured for dynamic updates? Clients pointed at the right DNS server?
* **External names failing:** forwarders configured correctly?
* **DNS auditing/debug logging** - per server, to a rotating text file, for queries, responses or both

## Best practices
* **More than one DNS server** (redundancy, fault tolerance)
* **Disable root hints**
* Use **secure forwarder partners**
* Enable **secure dynamic updates**

## References
* [DNS and AD DS](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/dns-and-ad-ds) · [AD-integrated zones](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/active-directory-integrated-dns-zones) · [Dynamic update](https://learn.microsoft.com/en-us/windows-server/networking/dns/dynamic-update) · [Troubleshoot DNS server](https://learn.microsoft.com/en-us/windows-server/networking/dns/troubleshoot/troubleshoot-dns-server)
* Exercises: *03a DNS Role Configuration*, *03b DHCP and DNS*
