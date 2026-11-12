---
course: SYSA1010
module: "Module 1 - Windows Server Deployment"
source: "Lesson 01a - Introduction to Windows Server.pptx"
tags: [SYSA1010, windows-server, virtualization]
---
# Introduction to Windows Server
Back to [[SYSA1010]] · Next: [[Windows Server Roles and Features]]

## Virtual machines, containers, hypervisors
| | Virtual machine (guest) | Container |
| --- | --- | --- |
| What | A computer inside a computer | An app + only the components it needs (microservice) |
| Size | Larger (full OS) | Smaller |
| Portability | Not easily portable | More portable |
| Startup | Slower | Faster |
| Security/isolation | Stronger | Weaker |

* **Hypervisor (host)** - hosts guest VMs
	* **Type 1** - guests run directly on hardware: **Hyper-V**, **Proxmox VE**, ESXi
	* **Type 2** - guests run through the host OS: **VMware Workstation**, **VirtualBox**
* See also [[Virtualization Concepts and Resource Allocation]] (SYSA1000)

## Windows Server editions
| Edition | VMs / Hyper-V containers per licence |
| --- | --- |
| **Standard** | 2 VMs, 2 Hyper-V containers |
| **Datacenter** | Unlimited |
| **Datacenter: Azure Edition** | Only runs in Microsoft Azure |

## Installation types
* **Desktop Experience** vs **Server Core** - **can't convert between them after install**, but they work together (a Desktop Experience server can manage a Core server)

| | Desktop Experience | Server Core |
| --- | --- | --- |
| Interface | GUI | **Command line only** (sconfig, PowerShell) |
| Roles/features | All | Most |
| Resources | Higher | Lower disk/RAM |
| Attack surface | Larger | **Smaller** |
| Use | Typical deployments, easier to manage | Where efficiency and security are priorities |

## Workgroup vs domain
| Workgroup | Domain |
| --- | --- |
| Decentralized - each system managed separately | **Centralized** users and policies |
| Accounts unique to each computer | Central server = **Domain Controller** running **Active Directory Domain Services (AD DS)** |
| Limited security policy control | Needs consistent connectivity to the DC |
| Home networks, closed-loop networks | Businesses (Microsoft 365 / Entra ID can replace or complement AD DS) |

## Getting Windows Server ISOs
* `.iso` = disk image in ISO 9660 format
* Via your licence portal, **Azure for Students**, or the **Microsoft Evaluation Center** (https://www.microsoft.com/en-us/evalcenter/)

## References
* [Windows Server editions comparison](https://learn.microsoft.com/en-us/windows-server/get-started/editions-comparison?pivots=windows-server-2019)
* [Azure Edition](https://learn.microsoft.com/en-us/windows-server/get-started/azure-edition)
* [What is Server Core](https://learn.microsoft.com/en-us/windows-server/administration/server-core/what-is-server-core)
* Exercises: *01a Environment Preparation*, *01b Deploy Windows Server with Desktop Experience*
