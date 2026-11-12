---
course: SYSA1010
module: "Module 1 - Windows Server Deployment"
source: "Lesson 02b - DHCP Server Role.pptx"
tags: [SYSA1010, windows-server, dhcp]
---
# DHCP Server Role
Back to [[SYSA1010]] · Prev: [[Windows Server Roles and Features]] · Next: [[DNS Server Role]]

> [!tip] Protocol details (DORA, leases, troubleshooting) are in [[DHCP]] (NETW1111)

## IP addresses
* Unique identifier on a network - IPv4 (`192.168.204.10`) or IPv6 (`2001:db8:85a3::8a2e:370:7334`)
* **Static** (manually configured) or **dynamic** ("borrowed" via DHCP)

## DHCP
* Saves configuration time, reduces errors, lets large networks reuse addresses
* Runs on a server, router, or elsewhere on the network
* Leases have a set length, can be **released** or **renewed**
* Hands out **IP address, subnet mask, default gateway/router, DNS server(s)**
* `ipconfig /all` shows *DHCP Enabled*, *DHCP Server*, *Lease Obtained*, *Lease Expires*

## Deploying the DHCP role
1. **Plan** subnets/VLANs - business case, review infrastructure, decide where DHCP lives
2. Give the server a **static IP**
3. **Install the DHCP Server role** (include management tools)
4. Run the **post-install wizard** (creates the DHCP security groups, authorizes in AD if domain-joined)
5. **Configure scopes** in the DHCP console
6. **Verify** the scopes

```powershell
Install-WindowsFeature DHCP -IncludeManagementTools
Add-DhcpServerv4Scope -Name "Clients" -StartRange 192.168.204.100 -EndRange 192.168.204.200 -SubnetMask 255.255.255.0
Set-DhcpServerv4OptionValue -ScopeId 192.168.204.0 -Router 192.168.204.1 -DnsServer 192.168.204.10 -DnsDomain "example.local"
Add-DhcpServerv4ExclusionRange -ScopeId 192.168.204.0 -StartRange 192.168.204.100 -EndRange 192.168.204.110
Add-DhcpServerv4Reservation -ScopeId 192.168.204.0 -IPAddress 192.168.204.150 -ClientId "00-0C-29-AA-BB-CC"
Get-DhcpServerv4Lease -ScopeId 192.168.204.0
```

## Managing scopes
* **Reservations** - fixed IP for a device (by MAC)
* **Lease range** and **exclusion range**
* **Scope options** vs **server options** (server options apply to all scopes; scope options override)
* **Deny** devices (filters)

## Best practices
* **One DHCP server per network** - multiple servers cause connectivity issues
* Monitor for **scope exhaustion**
* Monitor for **rogue DHCP servers**

## References
* [Install and configure DHCP Server](https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/quickstart-install-configure-dhcp-server)
* Exercise: *02b DHCP Role Configuration*
