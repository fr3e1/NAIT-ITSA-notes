---
cssclasses: [netw1111]
course: NETW1111
module: "Module 1 - Local Area Networks"
type: assignment
weight: "/20"
tags: [NETW1111, assignment, ipv4, ipv6, mac, tcp, udp]
---
# LAN Assessment
Back to [[NETW1111]]

> [!NOTE]
> **Overview**
> Short-answer + screenshot assessment built on the Module 1 exercises (*Testing Connectivity*, *MAC Addresses*, *TCP-IP*). Out of **20**.

## What it covers
| Exercise | Deliverable | Concepts |
| --- | --- | --- |
| Testing Connectivity | Ping between two **static IPv4** hosts; ping between two **static IPv6** hosts | [[LANs and IP Addressing]] |
| MAC Addresses | Custom (spoofed) MAC address + the DHCP lease it got | [[Ethernet, ARP and ICMP]], [[DHCP]] |
| TCP-IP | PortQryUI + PowerShell connection to a custom TCP port | [[TCP-IP and Transport Protocols]] |

## Key takeaways
* `192.168.x.x /24` → **class C**, **private**
* `ping -6` forces IPv6; plain `ping` prefers whatever the OS resolves (usually IPv4 for IPv4 literals). `ping -4` forces IPv4
* IPv6 prefix for `2001:db8::/64`-style lab addresses = the first 64 bits (`2001:db8::` with the zero hextets compressed)
* **MAC spoofing** risk: impersonate a trusted device - e.g. bypass MAC filtering / captive portals on public Wi-Fi, hijack sessions, evade bans
* TCP and UDP have **separate port spaces** - two apps can listen on the same number if one is TCP and one is UDP
* Protocols that use the same port on **both** TCP and UDP: **DNS (53)**, **Kerberos (88)**, **LDAP (389)**, HTTP/3 uses **UDP 443** alongside HTTPS on TCP 443
* `Test-NetConnection` only tests **TCP**; for UDP use tools like `nmap -sU` or a custom PowerShell `UdpClient` script
* Choose **UDP over TCP** when low latency matters more than reliability, or the app tolerates loss (games, live video, VoIP), or for simple request/response (DNS, DHCP)

## Useful commands
```powershell
ping 192.168.204.10
ping -6 2001:db8::10
Test-NetConnection -ComputerName 192.168.204.10 -Port 5000
Get-NetAdapter | Select Name, MacAddress
netstat -ano | findstr LISTENING
```
