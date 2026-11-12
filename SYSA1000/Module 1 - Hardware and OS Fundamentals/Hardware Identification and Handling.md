---
course: SYSA1000
module: "Module 1 - Hardware and OS Fundamentals"
source: "SYSA1000 - HW and OS Fundamentals - Hardware Identification and Handling.pptx"
tags: [SYSA1000, hardware, esd]
---
# Hardware Identification and Handling
Back to [[SYSA1000]] · Prev: [[Course Introduction]] · Next: [[HW and OS Fundamentals|Firmware, Compatibility, and Power Settings]]

* Every desktop is the same short list: **motherboard, CPU, RAM, storage, power supply** + expansion (graphics, network, AI accelerators)
* Each part has **one job** and **one way it connects**

## Components
| Component | Job | Notes |
| --- | --- | --- |
| **Motherboard** | Board everything connects to; carries signals between parts | CPU socket only fits **one CPU family** |
| **CPU** | General work of the computer | Sits in a socket under a clamp; contacts on the underside are **extremely easy to damage**. Architectures: **x86-64**, **ARM** |
| **RAM** | Short-term working space for what's running now | **Volatile** - empties on power loss. Clips in at both ends. Desktop = **DIMM**, laptop = **SODIMM** |
| **Storage** | Keeps data with the power off | **HDD** (spinning, cheapest/GB, slowest), **SSD** (no moving parts, faster, pricier), **NVMe** (M.2 slot on the board, no cables) |
| **Power supply (PSU)** | Converts wall power for components | Separate connectors for board, CPU, drives, graphics |
| **Cooling** | Moves heat off the CPU and out of the case | PSU + cooling are most overlooked and cause the **oddest faults** |
| **GPU** | Specialized maths - graphics, data analysis, AI | |
| **NIC** | Network connection, wired or wireless | |
| **NPU** | Newer accelerator for AI tasks | |

## How components connect
* **Sockets** - CPU drops in and clamps down
* **Slots** - RAM and expansion cards push in and click
* **Cables** - SATA drives need **data** and **power** cables separately
* Everything is **keyed** - only fits one way round

## Safe handling and ESD
> [!warning] Electrostatic discharge (ESD)
> Static destroys components and you **can't see the damage**.
> * Power off and **unplug** before opening a case
> * **Touch bare metal on the case** to ground yourself first (or wear an ESD strap)
> * Hold boards and cards **by the edges** only

## Links
* [Optiplex 7020 tear-down/rebuild](https://www.youtube.com/watch?v=gIpuDZAAtX0)
* See [[Assignment 1 - Hardware Fundamentals]]
