---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 2 - Virtualization"
source: "SYSA1000 - Virtualization - Virtual Machine Operations.pptx"
tags: [SYSA1000, virtualization, vmware, snapshots]
---
# Virtual Machine Operations
Back to [[SYSA1000]] · Prev: [[Virtualization Concepts and Resource Allocation]] · Next: [[Deployment Planning and Installation Media]]

## Appliances (OVF / OVA)
* An **appliance** = a complete VM packaged into a file
* **OVF** - open standard package (folder of files) · **OVA** - the same, as a single (tar) file
* **Importing** converts it to Workstation's format, checks it's valid and the hardware is supported → an ordinary VM folder

## Snapshots
* Preserve the machine **exactly at one moment** - **memory, settings, and every virtual disk**
* Machine keeps running; you can revert to that point repeatedly
* Taking one writes new files into the folder: the old disk is preserved and later changes go to a separate delta disk
* Cost space and speed → not meant to live forever

> [!WARNING]
> **A snapshot is NOT a backup**
> It lives in the same folder on the same disk. If the folder or drive is lost, the snapshots go with it. A **backup is a copy that lives somewhere else**.

## Clones
| | Full clone | Linked clone |
| --- | --- | --- |
| What | Complete, independent copy | Shares the original's disks |
| Depends on original? | No | **Yes** - breaks if the original is moved, changed or deleted |
| Use | Anything you keep | Quick throwaway tests only (**course says avoid**) |

* A clone copies the original's **identity** (name, MAC, Windows SID), not just files - see [[Deployment at Scale#Sysprep makes a copy safe]]

## Moving a VM to another computer
* **Export to OVF**, or just **copy the folder**
* **Compress (zip) the folder first** - one file survives transfer better

## Network modes
| Mode | Behaviour | VMware default |
| --- | --- | --- |
| **NAT** | Shares the host's connection to the outside | vmnet8 |
| **Bridged** | Gets its own address from the real network | vmnet0 |
| **Host-only** | Can reach the host and nothing else | vmnet1 |

See [[Initial setup]] for the program's vmnet configuration.
