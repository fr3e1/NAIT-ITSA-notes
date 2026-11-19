---
cssclasses: [sysa1010]
course: SYSA1010
module: "Module 3 - Server Documentation"
type: assignment
tags: [SYSA1010, assignment, documentation, onenote]
---
# Assignment 3 - Server Documentation
Back to [[SYSA1010]]

> [!NOTE]
> **Objective**
> Document a previously **undocumented server** (downloaded from ITSA SharePoint) in the class **OneNote template**, then export it to PDF.

## Setup
1. Power on the VM - a logon script asks for your name to build your unique server
2. **Note the username and password** shown on screen
3. Enter your full name → confirm initials (`Y` + Enter) → 10-second cancel window
4. ⚠️ **Don't touch the server** until every configuration phase finishes (several reboots, can take a while)
5. Log in with the noted credentials

## What to document
* Rename the *Server Overview* section to include your name + section number
* Server name
* Root domain name
* Domain name + **NetBIOS** name
* **Roles** installed
* IP addressing (address, mask, default gateway)
* **AD OU structure**
* **Domain controllers** (FQDNs)
* **FSMO role holder(s)**
* **Configuration of each installed role** (DHCP scopes/options, DNS zones/forwarders, etc.)

## Discovery commands
```powershell
hostname; Get-ComputerInfo | Select CsName, CsDomain, OsName, OsVersion
Get-NetIPConfiguration
Get-WindowsFeature | Where Installed
Get-ADDomain | Select DNSRoot, NetBIOSName, Forest, DomainMode
Get-ADForest | Select ForestMode, SchemaMaster, DomainNamingMaster
netdom query fsmo
Get-ADDomainController -Filter * | Select HostName, IPv4Address, Site
Get-ADOrganizationalUnit -Filter * | Select DistinguishedName
Get-DhcpServerv4Scope; Get-DhcpServerv4OptionValue -ScopeId <id>
Get-DnsServerZone; Get-DnsServerForwarder
Get-SmbShare
```

## Submission
* OneNote → **File → Export → Notebook → PDF**
* Name: `SYSA1010 Server Documentation <First Last>.pdf`

Related: [[Server Documentation]]
