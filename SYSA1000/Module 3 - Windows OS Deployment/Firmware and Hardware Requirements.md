---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 3 - Windows OS Deployment"
source: "SYSA1000 - Windows Deploy - Firmware and Hardware Requirements.pptx"
tags: [SYSA1000, firmware, uefi, tpm, secure-boot]
---
# Firmware and Hardware Requirements
Back to [[SYSA1000]] · Prev: [[Deployment Planning and Installation Media]] · Next: [[Installing Windows]]

* Every Windows 11 requirement has a value you can **read** - mostly in **firmware**, readable with no OS installed
* Reading first turns a day of work into five minutes of checking

## Where the values live
| Value | Usually under |
| --- | --- |
| Firmware mode (UEFI / legacy BIOS) | Boot |
| Secure Boot (on / off / unavailable) | Security |
| TPM (present/absent + version) | Security |
| Processor model | Main / system information page |

## Settings vs facts
| Settings (changeable) | Facts (fixed) |
| --- | --- |
| Firmware mode | Processor model |
| Secure Boot | TPM version |
| → machine can be brought up to standard | → machine has to be **replaced** |

## UEFI vs legacy BIOS
* Legacy BIOS (early 1980s) was **replaced, not improved**
* **UEFI** reads a partition table (**GPT**) → no 2 TB disk limit
* UEFI has a **boot manager** with named boot entries
* **Secure Boot is a UEFI feature** - legacy BIOS has nothing to enable

## Boot order vs one-time boot menu
* **Boot order** - saved list used every boot (changing it changes the machine)
* **One-time boot menu** (F12 on Dell) - picks a device for this boot only → **almost always the right tool**

## Reading values from inside Windows
```powershell
msinfo32          # System Information: "BIOS Mode" (UEFI/Legacy) and "Secure Boot State"
tpm.msc           # TPM Management: presence + "Specification Version"
Confirm-SecureBootUEFI
Get-Tpm
```

> [!IMPORTANT]
> **Firmware settings survive a reimage**
> Wiping replaces the **drive**; firmware lives on the **board**. A misconfigured firmware stays misconfigured through a full wipe - that's why **restoring defaults** is a real step.

## Watching it refuse
* The installer checks hardware before writing anything
* "This PC can't run Windows 11" is **not a fault or bad download** - it's working as designed

See also: [[HW and OS Fundamentals]] (firmware basics)
