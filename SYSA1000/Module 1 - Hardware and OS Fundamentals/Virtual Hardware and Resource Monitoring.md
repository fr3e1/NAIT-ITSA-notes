---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 1 - Hardware and OS Fundamentals"
source: "SYSA1000 - HW and OS Fundamentals - Virtual Hardware and Resource Monitoring.pptx"
tags: [SYSA1000, virtualization, vmware, monitoring]
---
# Virtual Hardware and Resource Monitoring
Back to [[SYSA1000]] · Prev: [[HW and OS Fundamentals|Firmware, Compatibility, and Power Settings]] · Next: [[Virtualization Concepts and Resource Allocation]]

* A **virtual machine** is a computer defined entirely by settings
* Every setting maps to a physical part you already identified - **the idea the whole course is built on**

## Hypervisors
* Software that creates and runs VMs, dividing one machine's resources among several
* **Type 1** - runs directly on hardware (ESXi, Hyper-V)
* **Type 2** - runs on top of an OS (VMware Workstation)

## Physical → virtual
| Physical | Virtual | Notes |
| --- | --- | --- |
| CPU | **vCPU** | Cores you assign. **Shared** with the host, not taken. More isn't automatically faster; too many slows everything |
| RAM | **vRAM** | **Taken** from the host while the VM runs. Too little = guest crawls/won't boot; too much = host struggles |
| Drive | **Virtual disk** | A **file** on your real drive. Grows as needed or allocated up front. **Delete the file = delete the machine** |
| NIC | **Virtual adapter** | Share host's connection (NAT), appear as its own machine (bridged), or isolated (host-only) |

## Monitoring
* **Task Manager in the guest** = what the guest *thinks* it has/uses
* **Task Manager on the host** = the **real** cost
* **VMware** shows what the VM is consuming
* The views rarely agree - *that's the lesson*

## Lab
* Build a VM from your **Hardware Passport** specs (vCPU, vRAM, disk), named with your student ID, pointed at a live Linux ISO (don't install)
* Then build one with **no installer** and start it → it fails. *Hardware without an OS does nothing*

## References (from slides)
* What is virtualization
* Introduction to virtualisation (Ubuntu Server docs)
* Hyper-V overview
* Difference between Type 1 and Type 2 hypervisors
* Creating virtual machines in VMware Workstation Pro
