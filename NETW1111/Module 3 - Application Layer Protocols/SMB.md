---
cssclasses: [netw1111]
course: NETW1111
module: "Module 3 - Application Layer Protocols"
lesson: "M3 Day 7 - SMB"
source: "SMB.pptx"
tags: [NETW1111, networking, smb, file-sharing]
---
# SMB
Back to [[NETW1111]] · Prev: [[DHCP]] · Next: [[Firewall Lab]]

* **Server Message Block** - Windows file and printer sharing
* Created by **IBM in 1983** for PC-DOS; adopted by Windows in **1987** over **NetBIOS**
* Used in this course as an example of problems passing data across networks/firewalls

## SMB on the LAN
* Provides name resolution (not DNS), share access and printer sharing
* File Explorer can browse non-hidden shares
* **Hidden shares end in `$`** (e.g. `C$`, `ADMIN$`)
* Designed for the LAN - most reliable there

## Across networks and firewalls
* **Share browsing stops working** across routers - it uses **broadcasts**
* Microsoft lists many ports: **135-139** (TCP+UDP) and **445** (TCP+UDP) → 10 exceptions on a NAT firewall
* **Port 445** = SMB directly over TCP, added later to work through firewalls - the crucial one
* 135-139 are only needed for **legacy** clients → open only 445 to reduce attack surface (NetBIOS doesn't route anyway)
* NetBIOS **names** don't route either → use **DNS** (or WINS) or the server's **IP** (`\\192.168.1.10\share`)

> [!WARNING]
> **Don't expose 445 to the Internet**
> Generally not secure practice (Microsoft's Sysinternals share is a rare exception).

## Versions
| Version | Year | Notes |
| --- | --- | --- |
| IBM SMB | 1983 | No version |
| SMB 1.0 | 1987 | NetBIOS-based. **Not recommended, disabled by default** |
| CIFS | 1996 | Rename (still seen in Linux land). Port 445 introduced |
| SMB 2.0 | 2006 | Better performance, fewer broadcasts, better security |
| SMB 3.0 | 2012 | More security, data center performance |

## Samba
* Open source (since 1991) reverse-engineered SMB for Linux/Unix
* File + printer sharing; can emulate a Windows 2008 domain controller
* URLs look like `smb://server/share`

```powershell
net view \\server           # list shares
net use Z: \\server\share   # map a drive
Get-SmbShare                # list local shares (PowerShell)
```

## Links
* [SMB](https://en.wikipedia.org/wiki/Server_Message_Block) · [NetBIOS over TCP/IP](https://en.wikipedia.org/wiki/NetBIOS_over_TCP/IP) · [Direct hosting of SMB over TCP/IP](https://docs.microsoft.com/en-us/troubleshoot/windows-server/networking/direct-hosting-of-smb-over-tcpip)
* Activity: *Exercise - SMB*
* See also: [[File Permissions and Network Shares]] (SYSA1010)
