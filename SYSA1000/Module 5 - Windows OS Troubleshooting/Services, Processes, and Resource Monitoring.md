---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 5 - Windows OS Troubleshooting"
source: "SYSA1000 - Windows Troubleshoot - Services, Processes, and Resource Monitoring.pptx"
tags: [SYSA1000, windows, troubleshooting, services, processes]
---
# Services, Processes, and Resource Monitoring
Back to [[SYSA1000]] · Prev: [[Windows Updates, Backup, and Restore]] · Next: [[Troubleshooting Performance and Startup]]

* Module 2 asked *how big* a machine is; this asks **what on the machine is using it**
* Configuration is done when the setting is right; **troubleshooting is done when you know why**
* Every tool answers: *what is running, and what started it?*

## Processes
* A running program with its own memory, identity and ≥1 thread
* Has a **PID** (process identifier)
* Started by a user, another process, or the system
* Task Manager: **Processes** tab (friendly) / **Details** tab (PIDs, numbers)

## Services
* Programs run by the **Service Control Manager (SCM)**, not a person - run even with **nobody signed in**
* SCM starts at boot and keeps the database of installed services
* A service is still a process - shows in both lists

| Display name | Service name | Job |
| --- | --- | --- |
| DHCP Client | `Dhcp` | Gets and renews the IP lease |
| DNS Client | `Dnscache` | Resolves and caches names |
| Windows Time | `W32Time` | Clock sync |
| Windows Defender Firewall | `MpsSvc` | Enforces firewall rules |
| Windows Search | `WSearch` | Indexing |
| Print Spooler | `Spooler` | Print queue |
| Task Scheduler | `Schedule` | Scheduled tasks (incl. at sign-in) |
| Windows Update | `wuauserv` | Updates |

### Startup types
| Type | Behaviour |
| --- | --- |
| **Automatic** | At boot, before sign-in |
| **Automatic (Delayed Start)** | Shortly after the other automatic services |
| **Manual** | Only when asked (user, program, dependent service) |
| **Disabled** | Can't start until the type is changed |

### Managing services
```powershell
services.msc                                   # console: Properties → General → startup type, Start/Stop
sc query Spooler
sc config Spooler start= demand                 # auto | delayed-auto | demand | disabled (space after = !)
Get-Service Spooler; Set-Service Spooler -StartupType Manual; Restart-Service Spooler
```
* Every tool writes the same `Start` value in the registry: `HKLM\SYSTEM\CurrentControlSet\Services\<name>`
* Startup type changes are logged: **Event Viewer → Windows Logs → System**, source **Service Control Manager** (event **7040**) - which service, old type, new type, when

## Reading CPU, memory, disk
* **Task Manager → Performance** - one graph per resource
* **Resource Monitor** (`resmon`) - Overview, CPU, Memory, Disk, Network tabs
* CPU **consistently > 80%** = problem; short spikes are normal
* **Disk 100%** with nothing open = something is reading/writing

### Disk full (space, not activity)
* Settings → System → Storage (by category)
```powershell
dir C:\ /s /a /o-s                                       # cmd: all files, largest first
Get-ChildItem C:\ -Recurse -Force -File -ErrorAction SilentlyContinue |
  Sort-Object Length -Descending | Select -First 20 FullName, Length
```

## Attributing a reading to a process
* Sort by the resource → top row is the suspect
* `tasklist /v` - PID, account, CPU time
* **Process Explorer** (Sysinternals) - adds the **command line**: what it runs, and from where
* > A name isn't an explanation until you know **what started it**

## When Task Manager is disabled
* Policy **Remove Task Manager** ("Task Manager has been disabled by your administrator")
* Set in `gpedit.msc` (User Configuration → Administrative Templates → System → Ctrl+Alt+Del Options), stored in the registry (`...\Policies\System\DisableTaskMgr`)
* Other tools still work: Resource Monitor, `tasklist`, Process Explorer

## Ending a process isn't fixing it
* `taskkill /PID 1234 /F` or `taskkill /IM name.exe` only ends it
* If something starts it, it's back next sign-in/boot. Three things start processes:
	1. **Services**
	2. **Startup apps**
	3. **Scheduled tasks** (Task Scheduler - trigger can be *At log on*)
