---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 4 - Windows OS Configuration"
type: assignment
tags: [SYSA1000, assignment, windows, permissions, firewall, networking, restore]
---
# Assignment 4 - Windows OS Configuration
Back to [[SYSA1000]]

> [!NOTE]
> **Objective**
> Configure a Windows 11 client for a network: accounts, permissions, firewall, addressing, updates and recovery. Builds on the VM from [[Assignment 3 - Windows OS Deployment]].

## Part 1 - Accounts and permissions
1. Settings → Accounts → **Other users** → add a **local** account named after your NAIT login, leave it **Standard User** (screenshot the *Change account type* dialog)
2. **Sign in as it once** - the first sign-in creates the profile
3. Create `C:\<login>\` with `<First>-Public` and `<Last>-Private` subfolders
4. On each subfolder: Properties → Security → **Advanced** → **Disable inheritance** → *Remove all inherited permissions*. **Don't Apply yet** (an empty saved ACL locks everyone out - fix with *Take ownership*)
	* **Public:** you = **Modify**, second account = **Read**
	* **Private:** you = **Modify**, nothing else
	* No Full control, **no Deny** (no entry = refused anyway)
5. Test **as the second account**: `whoami` + try to open Private → "You don't currently have permission". **Don't click Continue** (that's the admin elevation that would grant access)

> [!WARNING]
> Testing as yourself proves nothing - you're an admin and can open anything.

## Part 2 - Firewall rule for ping
* `ipconfig` on the VM → note the IPv4 address
* *Windows Defender Firewall with Advanced Security → Inbound Rules → New Rule* → **Custom** → All programs → **ICMPv4** → **Echo Request** only → any addresses → **Allow** → **Domain + Private + Public** → name `<login> - Allow Ping`
* Ping the VM **from the laptop** (4 replies) beside the Inbound Rules list
* Screenshot the rule's **General**, **Protocols and Ports**, **Advanced** tabs
* If ping already worked before the rule → something else allows it (usually *File and Printer Sharing (Echo Request)*). Disable it, confirm ping fails, then write yours
* Leave the rule in place for Part 3

## Part 3 - Addressing and name resolution
* `ipconfig /all` - address, mask, gateway, DNS, DHCP enabled, DHCP server
* Run in order:

| Test | Proves when it succeeds |
| --- | --- |
| `ping <gateway>` | Local network + gateway reachable |
| `ping 8.8.8.8` | Routing to the internet works |
| `ping dns.google` | Name resolution works |
| `nslookup nait.ca` | The configured DNS server answers directly |

* If `8.8.8.8` works but names don't → network is fine, **DNS** isn't (different repair). A failure recorded correctly earns full marks
* Ping the VM from the laptop again (from the other direction)

## Part 4 - Updates and restore
1. Windows Update → install everything → **Update history** screenshot
2. Search *Create a restore point* → System Protection → C: → **Configure** → turn on, give it a few % of disk
	* ⚠️ **Not** Settings → Recovery → *Point-in-time restore* (different feature that rolls back files too)
3. **Create** a restore point `<login> - before 7-Zip`
4. Install **7-Zip** from 7-zip.org
5. **System Restore** → pick your point → **Scan for affected programs** (7-Zip listed) → run → restart → completion message, 7-Zip gone

> [!NOTE]
> **Restore point ≠ snapshot ≠ backup**
> Restore point = inside Windows, OS state only. Snapshot = outside Windows, whole machine. Backup = separate copy of data elsewhere - the only one that survives losing the machine.

Related: [[User Accounts, Profiles, and Permissions]], [[Command Line, Firewall, and System Settings]], [[Network Addressing and Name Resolution]], [[Windows Updates, Backup, and Restore]]
