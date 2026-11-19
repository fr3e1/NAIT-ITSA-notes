---
cssclasses: [netw1111]
course: NETW1111
module: "Module 3 - Application Layer Protocols"
type: assignment
weight: "/10"
tags: [NETW1111, assignment, http, ftp, dns, dhcp, smb]
---
# App Layer Assessment
Back to [[NETW1111]]

> [!NOTE]
> **Overview**
> Screenshot + short-answer assessment built on the Module 3 exercises (HTTP, FTP, DNS, DHCP, SMB). Out of **10**.

| # | Task | Related |
| --- | --- | --- |
| 1 | Custom IIS default page (CustomPNG) | [[HTTP]] |
| 2 | Why doesn't the new image load? → the file was actually named `iisstart.png.png` (Explorer hides known extensions - turn on *File name extensions*) | [[HTTP]] |
| 3 | Failed FTP login showing the password in clear text in a capture (CatchMe) | [[FTP]], [[Packet Sniffing]] |
| 4 | Show **Passive FTP** in the FileZilla log → set the transfer mode to *Passive* in Site Manager / settings; the log shows `PASV` / `Entering Passive Mode` | [[FTP]] |
| 5 | nslookup of a custom hostname | [[DNS]] |
| 6 | Create a **TXT** record `AppLayerQ6` in the reverse zone `1.168.192.in-addr.arpa` and `nslookup -type=TXT` it | [[DNS]] |
| 7 | Failed `ipconfig /renew` with the router off | [[DHCP]] |
| 8 | Router off, both VMs on vmnet4 get **APIPA** addresses; disable IPv6, allow ICMPv4, ping the server **by hostname** | [[DHCP]], [[DNS]] |
| 9 | How does the client resolve the name? → No DNS, so Windows falls back to **LLMNR / NetBIOS** (multicast/broadcast on the local link) | [[DNS#Other name resolution (LAN only)]] |
| 10 | Access the `C$` hidden share through the router's WAN IP | [[SMB]] |

## Handy commands
```powershell
nslookup -type=TXT AppLayerQ6.1.168.192.in-addr.arpa 192.168.1.2
Add-DnsServerResourceRecord -ZoneName "1.168.192.in-addr.arpa" -Txt -Name "AppLayerQ6" -DescriptiveText "text"
ipconfig /release; ipconfig /renew
New-NetFirewallRule -DisplayName "ICMPv4 In" -Protocol ICMPv4 -IcmpType 8 -Action Allow
dir \\192.168.202.X\C$
```
