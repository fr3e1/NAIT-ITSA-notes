---
course: SYSA1000
module: "Module 4 - Windows OS Configuration"
source: "SYSA1000 - Windows Config - Command Line, Firewall, and System Settings.pptx"
tags: [SYSA1000, windows, powershell, firewall, bitlocker]
---
# Command Line, Firewall, and System Settings
Back to [[SYSA1000]] · Prev: [[User Accounts, Profiles, and Permissions]] · Next: [[Network Addressing and Name Resolution]]

## Why a command line?
* The GUI and CLI do the **same jobs**, but a command can be written down, repeated, and run on 100 machines
* Some machines (Server Core) have **no desktop**
* This course *shows* the CLI and *assesses* the GUI

## Two shells
* **Command Prompt (cmd)** - older; `ipconfig`, `ping`, `net`
* **PowerShell** - newer; **cmdlets** named **Verb-Noun** (`Get-LocalUser`)
* Many old commands run in both; PowerShell has equivalents for the rest

## Windows Defender Firewall
* **Inbound**: blocked unless a rule allows it
* **Outbound**: allowed unless a rule blocks it
* Replies to traffic the machine asked for are always allowed (stateful)

### Profiles
* Every network is **Domain**, **Private**, or **Public** - **new networks are Public by default**
* A rule only applies on its assigned profiles → **a rule on the wrong profile looks right and does nothing**

### What a rule matches on
* Direction (inbound/outbound)
* Program, protocol + port, or ICMP type
* Local and remote addresses
* Action (allow/block) and profile

### Why ping fails by default
* Ping = ICMP **Echo Request** (type 8) → **Echo Reply** (type 0)
* Windows ships rules for it, **disabled on Private and Public**; turning on file sharing enables one (the usual surprise)
* Writing your own rule is the correct (and assessed) way:
	* *Windows Defender Firewall with Advanced Security → Inbound Rules → New Rule → **Custom** → All programs → Protocol **ICMPv4** → Specific type **Echo Request** → Any addresses → Allow → all three profiles → name it clearly*

## Same job, two tools
| Task | GUI | cmd | PowerShell |
| --- | --- | --- | --- |
| Firewall rule | Advanced Security console | `netsh advfirewall firewall show rule name="Allow Ping"` | `Get-NetFirewallRule -DisplayName "Allow Ping"` + `Get-NetFirewallPortFilter` |
| BitLocker | Settings → Device encryption | `manage-bde -status` | `Get-BitLockerVolume` |
| Local accounts | Settings → Accounts → Other users | `net user` | `Get-LocalUser \| Format-List` (shows SID) |

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4 Echo" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -Action Allow -Profile Any
```

## Startup and behaviour
* **Startup apps** run as soon as you sign in
* Settings → Apps → Startup (toggle each) · Task Manager → Startup apps (adds **startup impact**)
* Fewer startup apps = first fix for a slow sign-in
