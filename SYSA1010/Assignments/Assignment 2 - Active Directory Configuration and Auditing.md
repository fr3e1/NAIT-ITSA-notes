---
course: SYSA1010
module: "Module 2 - Active Directory"
type: assignment
tags: [SYSA1010, assignment, active-directory, gpo, auditing]
---
# Assignment 2 - Active Directory Configuration and Auditing
Back to [[SYSA1010]]

> [!info] Objective
> Deploy a new **domain controller** (AD DS + DNS + DHCP) with **two Windows 11 clients**, a network share, and GPOs - applying **defense in depth** and **secure by design** - then audit account events.

## Infrastructure (all on **vmnet4** - host adapter + DHCP disabled)
| OS | VM / hostname | IP |
| --- | --- | --- |
| Windows Server 2019 Standard | `FirstName-DC1` | `192.168.204.<last2>` |
| Windows 11 Pro/Edu/Ent | `<Initials>Desk01` | DHCP |
| Windows 11 Pro/Edu/Ent | `<Initials>Desk02` | DHCP |
| pfSense | `SYSA1010-Router` | `192.168.204.254` |

* VM names in the library **must match** hostnames
* **Don't reuse** the A1 server
* Windows 11 needs Internet during install → use vmnet8 first, or give vmnet4 Internet via the router + your DC's DHCP/DNS
* Windows 11 **Home can't join a domain** - use Pro/Education/Enterprise

## Active Directory
* Domain: `FullName.SYSA<Term>` (e.g. `SYSA1261`), NetBIOS `FullName` (≤ 15 chars)
* **Enable the AD Recycle Bin**
* OUs: `FirstName`, `LastName`, `SysAdmins`
	* `SysAdmins` → named **Domain Admin** `FirstName.LastName` (display name includes "admin")
	* `FirstName` and `LastName` OUs → a **group** + a **user** of the same name, user added to the group

## DNS / DHCP
* Forwarders: CIRA Canadian Shield **Family** (IPv4 only) - `149.112.121.30`, `149.112.122.30`
* Default zones from domain creation
* DHCP same pattern as [[Assignment 1 - Windows Server Deployment#DHCP]], domain name option = `FullName.SYSA<Term>`

## Share
* `C:\<Initials>Shared` - **share:** Authenticated Users **Full** · **NTFS:** Domain Users **Read**, Domain Admins **Full**
* Subfolders `FirstName`, `LastName`

## GPOs
> Values from your ID: if the first digit used is `0`, replace it with `1`

**Default Domain Policy → Password Policy**
| Setting | Value |
| --- | --- |
| Enforce password history | 18 |
| Maximum password age | last 3 of ID (days) |
| Minimum password age | last 2 of ID (days) |
| Minimum length | 10 |
| Complexity | Enabled |

**Account Lockout Policy**
| Setting | Value |
| --- | --- |
| Lockout duration | last 3 of ID (min) |
| Lockout threshold | 10 |
| Reset lockout counter after | last 3 of ID (min) |
| Allow Administrator account lockout | Disabled |

* **Drive map GPO** - `S:` → `\\FirstName-DC1\<Initials>Shared`; only the FirstName and LastName users get it (User Config → Preferences → Windows Settings → Drive Maps; link to those OUs / security-filter to those groups)
* **Sleep GPO** linked to the **domain** - sleep timeout = last 4 of ID (Computer Config → Admin Templates → System → Power Management → Sleep Settings)
* Name GPOs with the class scheme, adding your initials to the *Users* / *Computers/Workstations* part

## Screenshots (17)
* **Server/AD (3):** Local Server (name, domain, IP, OS) · domain properties General tab (FQDN, NetBIOS, domain + forest functional level) · create user "FirstName Recycle Bin Test", delete it, show it in **ADAC → Deleted Objects** with the full **Last Known Parent** DN
* **OUs (4):** domain sorted by Type (OUs on top) · **Domain Admins** members (only you + Administrator) · contents of the FirstName OU · contents of the LastName OU
* **GPOs (5):** GPMC drive-map GPO scope/links · Default Domain Policy Settings tab (password + lockout) · Settings → Power on a client showing "managed by your organization" + timeout · `whoami` + `net use s:` as a mapped user · same as the named admin → *network connection could not be found*
* **Auditing (5):** lock out a user (screenshot the message) → unlock in ADUC → reset to a compliant password with *must change at next logon* → log in and change it → filter Security log for **4740, 4767, 4724, 4723** and show them **in chronological order** with message, subject, target account, workstation, event ID, computer, log name, source

```powershell
Install-WindowsFeature AD-Domain-Services, DNS, DHCP -IncludeManagementTools
Install-ADDSForest -DomainName "fullname.sysa1261" -DomainNetbiosName "FULLNAME" -InstallDns
Enable-ADOptionalFeature 'Recycle Bin Feature' -Scope ForestOrConfigurationSet -Target fullname.sysa1261
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4740,4767,4724,4723} | Sort TimeCreated
```

Related: [[Identity Management]], [[User Management]], [[Group Policy Objects]], [[Auditing Active Directory]]
