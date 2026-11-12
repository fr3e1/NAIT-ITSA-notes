---
course: SYSA1000
module: "Module 2 - Virtualization"
type: assignment
tags: [SYSA1000, assignment, virtualization, vmware, snapshots]
---
# Assignment 2 - Virtualization Configuration
Back to [[SYSA1000]]

> [!info] Objective
> Import a supplied VM, change its resources and record the difference, use snapshots and clones, and test the three network modes.

## Part 1 - Import and size
* **File → Open** `SYSA1000-Assignment2.ova` → name + location → import (unpack + convert takes a few minutes)
* Run it at three sizes (power **off** to change CPU/RAM), use it for a few minutes, record time-to-desktop, feel, and resource monitor readings:

| Run | Cores | Memory | Expect |
| --- | --- | --- | --- |
| 1 | 1 | 512 MB | Below Ubuntu's 2 GB minimum - **miserable on purpose**, may not reach the desktop, heavy swap |
| 2 | 1 | 2048 MB | Usable but laggy |
| 3 | 2 | 4096 MB | Comfortable for basic tasks |

* Compare the VM at run 3 to your laptop - every virtual component **comes from** a physical one (vCPUs from host cores, vRAM reserved from host RAM, virtual disk = file on host drive, NAT adapter shares the host's NIC)
* *Optional:* give it much more - the VM barely improves, but the **laptop** suffers
* Write down the VM's folder path, then **Remove from Library**

## Part 2 - Operations
1. **File → Open** the `.vmx` in the recorded folder (**don't re-import the OVA** - that makes a second machine). It returns exactly as left - *Remove from Library deletes nothing*
2. Take a snapshot named `Clean`
3. Run `assignment2-break-it.sh` (right-click → *Run as a program*, or `bash assignment2-break-it.sh` - **bash, not sh**). Screenshot the changed desktop with `CHANGED.txt` readable
4. **Revert to `Clean`** → `CHANGED.txt` is gone (a snapshot taken before the file existed can't contain it)
5. **Clone** it (full or linked) and explain: deleting the original **breaks a linked clone** (it depends on the original's disks); a **full clone** is independent

## Part 3 - Virtual networking
For each mode run `ip addr`, `ping <laptop IP>`, `ping 8.8.8.8`:

| Mode | Address from | Reach laptop | Reach internet | Use when |
| --- | --- | --- | --- | --- |
| **NAT** (vmnet8) | VMware's NAT DHCP (192.168.202.x) | ✅ | ✅ | VM needs internet but stays hidden behind the host |
| **Bridged** (vmnet0) | The **real** network's DHCP (same subnet as the laptop) | ✅ | ✅* | VM must act like any other LAN device |
| **Host-only** (vmnet1) | VMware host-only DHCP (192.168.201.x) | ✅ (via host adapter) | ❌ | Isolated lab network |

\* Bridged often **fails on campus/public networks** - it puts an unknown device directly on a network you don't control (extra MAC/DHCP request, often blocked by port security or Wi-Fi client isolation). A recorded failure is a correct answer.

* Finish: **Manage → Delete from Disk** for the VM and the clone (this is the one that actually frees space - check screenshots first!)

Related: [[Virtualization Concepts and Resource Allocation]], [[Virtual Machine Operations]]
