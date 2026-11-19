---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 2 - Virtualization"
source: "SYSA1000 - Virtualization - Virtualization Concepts and Resource Allocation.pptx"
tags: [SYSA1000, virtualization, vmware]
---
# Virtualization Concepts and Resource Allocation
Back to [[SYSA1000]] · Prev: [[Virtual Hardware and Resource Monitoring]] · Next: [[Virtual Machine Operations]]

## What a hypervisor does
* Makes **virtual hardware out of real hardware** - the guest can't tell the difference
* Nothing is invented: every virtual component is backed by a physical one
* **Type 1** - bare metal, no OS beneath (ESXi) · **Type 2** - an app on an OS (**VMware Workstation**)

## A VM is a folder of files
| File | Holds |
| --- | --- |
| `.vmx` | Settings - which virtual hardware and how much |
| `.vmdk` | Virtual hard disk contents |
| `.nvram` | Virtual firmware (BIOS/UEFI) settings |
| `.vmsd` / `.vmsn` | Snapshot metadata / state |
| `.log` | VM logs |

> [!TIP]
> Copy the folder and you've copied the machine

## The three things you allocate
### Processors
* **Number of processors × cores per processor** = total vCPUs
* The total can't usefully exceed the cores your laptop actually has

### Memory
* VMware's slider marks a **guest minimum**, **recommended**, and **maximum recommended**
* One VM can get up to 64 GB, but the real ceiling is your laptop's installed RAM

### Disk
* **Growable (thin)** - starts small, expands as the guest writes
* **Preallocated (thick)** - claims the full size immediately (faster, uses space up front)
* Either can be **one file** or **split into 2 GB pieces**

## Sizing against your laptop
* Everything the VM gets is **taken from the host**, not added
* Leave enough for the host OS - a VM given everything makes the laptop unusable
* Resources aren't interchangeable: lots of RAM + 1 core is still slow
* **Whatever is scarcest sets the speed** (the bottleneck moves as you change things)

See [[Assignment 2 - Virtualization Configuration]]
