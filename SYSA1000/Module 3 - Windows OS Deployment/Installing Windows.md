---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 3 - Windows OS Deployment"
source: "SYSA1000 - Windows Deploy - Installing Windows.pptx"
tags: [SYSA1000, windows, installation, vmware, tpm, bitlocker]
---
# Installing Windows
Back to [[SYSA1000]] · Prev: [[Firmware and Hardware Requirements]] · Next: [[Deployment at Scale]]

* A VM must meet the **same requirements** as physical hardware - Setup can't tell the difference
* On physical hardware failures may be unfixable; on a VM **every requirement is a setting**

## Build the baseline first
* Create the VM as a **Windows 10 guest** on purpose - BIOS firmware, no TPM, modest memory
* That's a known starting point that **fails** the Windows 11 check
* Every change after that is one you chose and can explain

## What has to change (and why)
| Change | Why |
| --- | --- |
| Memory ≥ **4 GB**, disk ≥ **64 GB** | Windows 11 minimums |
| Firmware **BIOS → UEFI** | Required; enables GPT and Secure Boot |
| **Secure Boot** enabled | Required; only possible with UEFI |
| Add a **TPM 2.0** device | Required |

> [!CAUTION]
> **Adding a vTPM encrypts the VM**
> Workstation must protect the TPM's keys, so it **encrypts the VM** and asks for a password. **There is no recovery for that password.** Write it down.

## The disk step
* Setup writes **several partitions** (EFI System, MSR, Windows, Recovery)
* **UEFI boot requires GPT** - an old BIOS/MBR layout will be refused
* Let Setup create the layout (delete existing partitions → *Next*) - almost always right

```cmd
:: Shift+F10 in Setup - convert a disk manually if needed
diskpart
list disk
select disk 0
clean
convert gpt
```

## What Setup actually does
1. Boots a small temporary OS (WinPE)
2. Partitions and formats the disk
3. Expands the Windows image onto it
4. Installs drivers for found hardware, restarts
→ one progress bar, four jobs

## First boot and the BitLocker trap
* OOBE asks for a computer name and an account
* Windows may turn on **device encryption** here
* The **recovery key is saved to the account used** - sign in with no (Microsoft) account and the key has nowhere to go

## Snapshot before anything else
* Take a **snapshot of the clean install** before the first update, setting or app
* That's why nobody has to install Windows twice in this course

See [[Assignment 3 - Windows OS Deployment]]
