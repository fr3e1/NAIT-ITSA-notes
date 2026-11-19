---
cssclasses: [netw1111]
course: NETW1111
module: "Module 3 - Application Layer Protocols"
lesson: "M3 Day 4 - FTP Protocol"
source: "FTP.pptx"
tags: [NETW1111, networking, ftp]
---
# FTP
Back to [[NETW1111]] · Prev: [[HTTP]] · Next: [[DNS]]

* **File Transfer Protocol** - transfers larger files, authenticated or anonymous
	* Distributing updates/software (usually anonymous)
	* Updating website content (authenticated)
* Uses **TCP 20 and 21** plus temporary high (unregistered) ports
* Long history of security compromises - partly popularity, partly architecture

## FTP vs HTTP
* [[HTTP]] is **stateless**; FTP keeps a **session** open and remembers state (e.g. current working directory)
* FTP lets you change options mid-session
* FTP connection setup is slower

> [!WARNING]
> **Not the same as FTP**
> * **SFTP** - part of **SSH**, port **22**, encrypted, firewall friendly, popular on Linux (see [[SSH]])
> * **TFTP** - **UDP**, unauthenticated; used for network boot images and switch/router OS upgrades on a LAN

## Features
* Authenticated or anonymous login
* Text (ASCII) or binary transfer modes
* Active or passive modes
* Encrypted or plain text

## Ports and modes
* **Port 21 = command channel** - client connects here, all commands go through it
* **Port 20 = data channel** in classic (active) FTP

| | Active FTP | Passive FTP |
| --- | --- | --- |
| Command channel | Client → server :21 | Client → server :21 |
| Data connection | **Server** (source port 20) → client's high port | Client sends `PASV`, server replies with a high port, **client** connects to it |
| Firewall friendly? | ❌ client firewalls block the inbound connection | ✅ client initiates everything |

* **NAT on both ends** makes dynamic ports hard → define a narrow passive port range on the server and forward it on the NAT firewall

## Servers and clients
* Servers: **IIS** (FTP role), **vsftpd** (Linux/Unix), **FileZilla Server**
* Clients: Windows built-in `ftp` (no passive mode!), FileZilla, WinSCP, Cyberduck, FireFTP, cURL, Linux `ftp`
* cURL is common for scripting anonymous downloads

## Authentication
* Username/password, or **anonymous** - username `anonymous`, password = your email (convention, not enforced)
* Anonymous servers are **read-only**

## Security
* Classic FTP sends **everything in clear text** - usernames, passwords and data
* **FTPS** = FTP + SSL/TLS
	* **Implicit FTPS** - TLS required immediately, insecure connections dropped (port **990**)
	* **Explicit FTPS (FTPES)** - client initiates security and makes most decisions
		* Can encrypt only the command channel (creds + commands) or the data channel too (full security, more CPU, slower)
		* Server and client can both be authenticated with certificates

## Firewall rules (passive FTP)
* Allow **21 in**, **20 out**, and the configured **passive port range in**
* Implicit FTPS: open **990** + the passive range

## Common commands
| Command | Action |
| --- | --- |
| `ls` | List current directory |
| `cd` | Change directory |
| `dele` | Delete file |
| `get` | Download server → client |
| `put` | Upload client → server |
| `pasv` | Switch to passive mode |
| `quit` | Close connection |
| `?` | List commands |

## Troubleshooting
| Symptom | Likely cause / fix |
| --- | --- |
| Login works but transfers time out | Active FTP can't reach the client → use passive (`PASV`); or firewall blocking high ports |
| FTPS server drops connection | Server wants **implicit**, client using **explicit** (or vice versa) - match the server's requirements |
| Can't upload | Anonymous = read-only; authenticated user also needs filesystem write permission |
| Certificate errors on FTPES | Expired cert, untrusted CA, or name mismatch (DNS CNAME). Verify, then accept temporarily/permanently |

## Links
* Video: [FTP, SFTP, TFTP Explained](https://www.youtube.com/watch?v=tOj8MSEIbfA)
* Activity: *Exercise - FTP*
