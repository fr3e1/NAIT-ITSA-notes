---
course: SYSA1010
module: "Module 1 - Windows Server Deployment"
type: assignment
tags: [SYSA1010, assignment, windows-server, dhcp, dns, shares]
---
# Assignment 1 - Windows Server Deployment
Back to [[SYSA1010]]

> [!info] Objective
> Build a **workgroup** with one **Windows Server 2019** (Desktop Experience) and one **Windows 11** client, configuring DHCP, DNS and file shares. Submitted as a Word/PDF report with a cover page and screenshots.

## Infrastructure
| OS | VM name | IP |
| --- | --- | --- |
| Windows Server 2019 Standard | `<Initials>-SRV1` | `192.168.204.<last2 of ID>` /24 |
| Windows 11 client (`ITSAProgram-Windows11-Unattended.iso`) | | DHCP |
| SYSA1010-Router | | provided, no changes |

* Placeholders: `FirstName`, `LastName`, `FullName`, `YourInitials` (e.g. Raymond Holt → `RH-SRV1`)
* Last 2 digits `00` → use `01`
* Both machines need **Internet** once DHCP/DNS are up
* Local users on the server: `FirstName`, `LastName`, `FullName`

## Server
* Desktop Experience, correct name, static IP, **correct time zone**, DHCP + DNS roles (best practice), updates installed (activation not required)

## DNS
* Client **and** server resolve against the server
* **Forwarders:** CIRA Canadian Shield **Protected** (IPv4 only) - `149.112.121.20`, `149.112.122.20`
* Forward zone `SYSA1010.FullName`, **nonsecure and secure dynamic updates** allowed:

| Type | Name | Value |
| --- | --- | --- |
| A | `<Initials>-SRV1` | `.<ID>` |
| A | `FirstName` | `.<ID+10>` |
| A | `LastName` | `.<ID+20>` |
| TXT | `FullName-ID` | Student ID |
| CNAME | `FullName` | `<Initials>-SRV1` |

## DHCP
* Scope name `FullName`, range `.<ID+50>` - `.<ID+100>`
* Lease = last 3 digits of the ID as **days / hours / minutes** (…456 → 4 d 5 h 6 min)
* Options: **003 Router** (router address), **006 DNS Servers** (the server), **015 Domain Name**

## Network share
* Add a **25 GB SATA** disk → format as **N:**, label `SYSA1010-FirstName`
* `N:\SYSA1010\` with a subfolder per user
* NTFS (remove everything else):
	* **Administrator** - Full control (can change permissions)
	* **Each user** - create/delete in **their own** folder, **can't change permissions** → *Modify*
	* **FullName** - Modify on the other folders too
* Two shares of `SYSA1010`:
	* **Visible** share - all users **Read**
	* **Hidden** share (`SYSA1010$`) - only Administrator and FullName can create/delete

```powershell
Initialize-Disk 1; New-Partition -DiskNumber 1 -UseMaximumSize -DriveLetter N | Format-Volume -FileSystem NTFS -NewFileSystemLabel "SYSA1010-First"
New-SmbShare -Name SYSA1010 -Path N:\SYSA1010 -ReadAccess Everyone
New-SmbShare -Name 'SYSA1010$' -Path N:\SYSA1010 -ChangeAccess FullName,Administrator
Get-SmbShare SYSA1010* | Get-SmbShareAccess
```

## Screenshots (17)
* **Server (3):** Server Manager Local Server (name, last updates, IP, time zone) · `ipconfig /all` · Update history (quality updates)
* **DNS (5):** resolve `FirstName`, `LastName`, `FullName`, `FullName-ID` **from the client without the FQDN** (works because DHCP option 015 sets the DNS suffix) · Forwarders tab
* **DHCP (3):** Scope Options (server + scope name + options) · scope properties (range + lease) · client `ipconfig /all`
* **Share (6):** `Get-SmbShare SYSA1010* | Get-SmbShareAccess` · advanced permissions of each of the 3 folders (owner + inheritance) · from the client, log in over SMB **as FullName** (don't remember credentials), create `<name>.txt` containing your ID in FullName's folder (VM name, path, contents and Date Modified visible) · file's advanced security (name + owner)

Related: [[DHCP Server Role]], [[DNS Server Role]], [[File Permissions and Network Shares]]
