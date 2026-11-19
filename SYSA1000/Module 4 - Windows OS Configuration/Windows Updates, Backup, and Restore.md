---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 4 - Windows OS Configuration"
source: "SYSA1000 - Windows Config - Windows Updates, Backup, and Restore.pptx"
tags: [SYSA1000, windows, updates, backup, restore]
---
# Windows Updates, Backup, and Restore
Back to [[SYSA1000]] · Prev: [[Network Addressing and Name Resolution]] · Next: [[Services, Processes, and Resource Monitoring]]

## Three ways to undo, at three layers
| | Snapshot | Restore point | Backup |
| --- | --- | --- | --- |
| Lives | **Outside** the OS (hypervisor) | **Inside** the OS | **Somewhere else** |
| Captures | Whole machine | System state (system files, programs, registry, settings) - **not your files** | Your data |
| Survives losing the machine? | ❌ | ❌ | ✅ **only this one** |

## Windows Update
* Updates fix problems someone **already knows how to exploit** - unpatched machines get broken into
* **Monthly** (Patch Tuesday) = security + quality fixes, **cumulative**
* **Yearly feature update** = changes the Windows version
* Settings → Windows Update: check, see pending, **Update history** (by kind, with result)
* **Active hours** stop restarts while you work; **Pause** holds updates only for a while

## Restore points (System Protection)
* Taken before changes or on request
* Hold system files, installed programs, registry, settings - **not documents** (and don't touch them on restore)
* **Off on a fresh install**, turned on **per drive**
* Steps: turn on protection for C: → **Create** (name it) → make a change (install a program) → **System Restore** shows affected programs → restarts and reports which point it used

```powershell
Enable-ComputerRestore -Drive "C:\"
Checkpoint-Computer -Description "Before app install"
Get-ComputerRestorePoint
rstrui.exe     # System Restore wizard
```

### Point-in-time restore ≠ System Restore
* Windows 11 feature that rolls back the **entire PC, files included**
* ~daily, kept 3 days, on by default on large unmanaged drives; under Settings → Recovery
* Closer to a snapshot - **still not a backup**

## Backups
* A second copy of the data on a **different disk or place** - a copy on the same disk is a convenience, not a backup
* > A backup that has never been checked is a **hope** - verify by opening the destination and finding the file
* **File History** - copies libraries + desktop to another drive/network location on a schedule; **refuses the same disk**. Restore via the file's **Previous Versions** tab
* **Windows Backup app** - different thing; syncs to the cloud with a personal Microsoft account

## When a restore point isn't enough
* **Reset this PC** - reinstalls Windows, keep or remove files
* **Windows Recovery Environment (WinRE)** - Startup Repair, uninstall updates, Command Prompt (used in Module 5 - [[Troubleshooting Performance and Startup]])
