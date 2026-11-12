---
course: SYSA1000
module: "Module 3 - Windows OS Deployment"
type: assignment
tags: [SYSA1000, assignment, windows11, uefi, tpm, vmware]
---
# Assignment 3 - Windows OS Deployment
Back to [[SYSA1000]]

> [!info] Objective
> Decide whether a physical desktop meets Windows 11 requirements, then convert a Windows 10 VM into one that does and install Windows 11.

## Part 1 - Can the classroom desktop run Windows 11?
* Read the **TPM version** in firmware (write it down)
* Use **Microsoft's own** Windows 11 requirements page (link it)
* Compare every row - **more than one row fails** on the lab desktops:

| Requirement | Windows 11 needs | Lab desktop (OptiPlex, i5-4590) |
| --- | --- | --- |
| TPM | **2.0** | 1.2 ❌ |
| Processor | 1 GHz+, 2+ cores, **on the supported CPU list** (Intel 8th gen+) | 4th gen ❌ |
| Secure Boot | Capable | ✅ |
| RAM | 4 GB | ✅ |
| Storage | 64 GB | ✅ |

* Conclusion: name **every** failure and whether it's fixable. TPM version and CPU generation are **facts, not settings** → the machine must be **replaced**. Bypass tools exist; unsupported installs get **no security updates**

## Part 2 - Convert a Windows 10 VM
* Create the VM with guest version **"Windows 10"** (not "Windows 10 x64"), name `SYSA1000-A3-<studentID>`, install nothing
* Screenshot **before** state: *Options → General* (name, guest OS + **Version**, working directory) and the *Hardware* tab

| Setting | Template | Change to | Requirement |
| --- | --- | --- | --- |
| Processors | 2 cores | (already OK) | 64-bit, 2+ cores |
| Memory | 1 GB | **4 GB** | 4 GB RAM |
| Disk | 60 GB | **64 GB** (looks close - isn't) | 64 GB storage |
| Firmware | BIOS | **UEFI** | UEFI |
| Secure Boot | off | **on** (only appears once UEFI is set) | Secure Boot |
| Encryption | none | **enabled** | *Not a Windows 11 requirement* - VMware requires it to protect the vTPM's keys |
| TPM | none | **TPM 2.0** (add **last**) | TPM 2.0 |

* Screenshot each change with the changed value **boxed** - a settings screenshot proves nothing unless the reader can tell what you touched
* ⚠️ The encryption password has **no recovery path** - store it safely

## Part 3 - Why won't the installer boot?
* Attach the ISO (stored locally, **not a OneDrive placeholder**), *Power On to Firmware* → Boot Manager → CD-ROM → press a key within ~5 s
* It **doesn't boot** - firmware skips the disc silently
* Compare against a fresh **Windows 11 x64** VM: the only difference is **Options → General → Version**
* **Cause:** plain "Windows 10" = **32-bit** → VMware built **32-bit UEFI** firmware into the `.nvram`. Windows 11 ISOs only have a **64-bit boot loader** → skipped. The `.nvram` is written once and **not rebuilt** when the Version changes
* **Fixes:**
	* Rebuild from the **Windows 10 x64** template and redo the six requirements, *or*
	* Change Version to **Windows 10 x64** **and** delete the `.nvram` (both are required; only safe with **nothing installed** - once Windows is installed the **vTPM keys live in the .nvram**)

> [!tip] Troubleshooting habit
> Compare against a known-good machine, find the one difference, and be able to *explain* the fault - not just follow a fix.

## Part 4 - Install, snapshot, evidence
* Install Windows 11; computer name includes your student ID; username `First.Last`
* Screenshot the VMware window with the Windows 11 desktop inside it
* **Snapshot immediately** after install, before any changes
```cmd
hostname & whoami & systeminfo | findstr /C:"OS Name" /C:"OS Version" /C:"Original Install Date"
```
* Screenshot the Snapshot Manager

Related: [[Deployment Planning and Installation Media]], [[Firmware and Hardware Requirements]], [[Installing Windows]]
