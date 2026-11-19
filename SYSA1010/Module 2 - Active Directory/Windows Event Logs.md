---
cssclasses: [sysa1010]
course: SYSA1010
module: "Module 2 - Active Directory"
source: "Lesson 08a - Windows Event Logs.pptx"
tags: [SYSA1010, event-logs, troubleshooting, auditing]
---
# Windows Event Logs
Back to [[SYSA1010]] · Prev: [[Group Policy Objects]] · Next: [[Auditing Active Directory]]

* Windows' central logging system - detailed records of system, security and application activity
* Used for **diagnostics**, **security auditing**, **compliance**, **forensics**
* View with **Event Viewer** (`eventvwr.msc`) or PowerShell

## Log types
| Log | Contains |
| --- | --- |
| **Application** | App startup/shutdown, app-specific events, crashes, runtime errors, role/feature issues (content chosen by developers) |
| **Security** | **Audit-driven, controlled by policy** - sign in/out, account management, privileged/object access (needs object-level auditing enabled), policy changes |
| **Setup** | Windows updates, feature upgrades, driver installs, role/feature changes (failed installs may land in Application instead) |
| **System** | Kernel, drivers, core services starting/stopping, shutdown status |
| **Forwarded Events** | Events sent from other machines to an event collector (resource heavy; largely replaced by other tools/SIEMs) |
| **Applications and Services Logs** | Verbose per-component logs (DNS Server, DHCP, Directory Service...). Check here + Application + System when troubleshooting roles |

## Limitations
* Logs have a **maximum size** (adjustable)
* **Circular** - oldest events are overwritten
* Logging costs resources; some logs **aren't enabled by default**

## Event structure
| Field | Notes |
| --- | --- |
| **Level** | Information, Warning, Error, Critical |
| **Date and Time** | When it was **recorded** (not created) |
| **Source** | Component that logged it |
| **Event ID** | 0-65535 |
| **Task Category** | |
| **Message** | Details |
| **Computer** / **User** | |

```powershell
Get-WinEvent -LogName System -MaxEvents 20
Get-WinEvent -FilterHashtable @{LogName='System'; Level=2; StartTime=(Get-Date).AddDays(-1)}   # errors last 24h
Get-WinEvent -ListLog * | Where RecordCount -gt 0
Get-EventLog -LogName Application -Newest 10 -EntryType Error   # older cmdlet
```

## References
* [Understanding the Windows Server event log](https://techcommunity.microsoft.com/blog/itopstalkblog/understanding-the-windows-server-event-log/4417350)
* [Windows Event Forwarding for intrusion detection](https://learn.microsoft.com/en-us/windows/security/operating-system-security/device-management/use-windows-event-forwarding-to-assist-in-intrusion-detection)
* Exercise: *08a Viewing Events*
