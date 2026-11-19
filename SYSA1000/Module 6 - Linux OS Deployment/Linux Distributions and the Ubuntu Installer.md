---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 6 - Linux OS Deployment"
source: "SYSA1000 - Linux Deploy - Linux Distributions and the Ubuntu Installer.pptx"
tags: [SYSA1000, linux, ubuntu, open-source, filesystems]
---
# Linux Distributions and the Ubuntu Installer
Back to [[SYSA1000]] · Prev: [[Troubleshooting Performance and Startup]] · Next: [[Installing Ubuntu and First Boot Configuration]]

* A VM's hardware is just settings - the same settings run Windows or Ubuntu
* The **OS is the layer that makes hardware usable, and it is replaceable**

## Open source
* **Open source is a licence, not a price** - anyone may read, change and share the source
* The **Open Source Definition** has 10 criteria; the first three: **free redistribution**, **source code available**, **derived works allowed**. "Free of charge" isn't one of them
* **GNU Project** (1984) - build a complete OS anyone could share
* **Free software** = four freedoms: **run**, **study and change**, **share**, **share your changes**
* "Open source" coined in **1998** to stress quality/practicality over ethics - same software, different reasons

## Distributions
* **Distro** = Linux **kernel** + **GNU tools** + **package manager** + **installer** + defaults
* The kernel is shared; the choices around it aren't. Anyone can assemble one (the licences allow it)

| Family | Examples | Package manager |
| --- | --- | --- |
| **Debian** | Debian, Ubuntu, Mint | `apt` (.deb) |
| **Red Hat** | RHEL, Fedora, Rocky, Alma | `dnf` (.rpm) |
| **SUSE** | openSUSE, SLES | `zypper` |
| **Arch** | Arch, Manjaro | `pacman` |
| **Slackware** | Slackware | `pkgtool` |
| **Gentoo** | Gentoo | `emerge` |

## Ubuntu releases
* Version = **year.month** → **26.04** = April 2026
* **LTS** - every 2 years, **5 years** of security updates
* **Interim** - every 6 months, **9 months** of updates
* Course uses **Ubuntu Desktop 26.04 LTS**

## The installer's disk step
* Windows Setup picks partitions for you; Ubuntu asks: **erase disk**, **install alongside**, or **manual partitioning**
* It also asks whether to **encrypt the disk** (Windows never asks)
* *Erase disk and install* → whole disk, **two partitions** (EFI System Partition + root)

## File systems
| FS | Notes |
| --- | --- |
| **ext4** | Ubuntu default; successor to ext3; **journaling** (records intended changes so a crash doesn't leave it half-changed) |
| **LVM** | Logical Volume Management - a layer between disk and FS; volumes can **grow while in use**, even across disks |
| **Btrfs** | Copy-on-write, built-in snapshots; default on Fedora Workstation and SUSE |
| **XFS** | Very large files/filesystems; default on RHEL |
| **ZFS** | Pooled storage across disks with self-repair; advanced option in Ubuntu's installer |
| **NTFS** | Windows' FS; Linux can read and write it |

## Filesystem hierarchy (FHS)
* Everything starts at **`/`** - one tree, **no drive letters**; other disks are **mounted** at a directory

| Path | Holds |
| --- | --- |
| `/etc` | Configuration |
| `/home` | Users' folders |
| `/var` | Data that changes (logs in `/var/log`) |
| `/boot` | Boot loader files, kernel |
| `/usr` | Programs |
| `/root` | root's home |
| `/tmp` | Temporary files |
| `/dev` | Device files (`/dev/sda`) |
* Universal across distributions

## Secure Boot and TPM
* Secure Boot works for Ubuntu - its first-stage loader (**shim**) is signed with a key firmware already trusts
* A **vTPM** is software in the VM's files - only as safe as the password on them
* VMware's Ubuntu template starts as **BIOS** → switch to **UEFI before installing**
