---
course: SYSA1000
module: "Module 3 - Windows OS Deployment"
source: "SYSA1000 - Windows Deploy - Deployment Planning and Installation Media.pptx"
tags: [SYSA1000, windows, deployment, iso, checksum]
---
# Deployment Planning and Installation Media
Back to [[SYSA1000]] · Prev: [[Virtual Machine Operations]] · Next: [[Firmware and Hardware Requirements]]

* **Deployment** = getting an OS onto a machine and making it usable
* **Installing** replaces what's on the disk; **upgrading** keeps it
* Both start with: *can this machine run it at all?*

## Windows 10 end of support
* Ended **October 2025** - no feature updates, fixes or security updates
* Paid **Extended Security Updates (ESU)** buy time, not a solution

## Installation media
* **Microsoft** publishes Windows media free; **Canonical** publishes Ubuntu
* Media from anywhere else **can't be verified as unmodified**
* The media is free - **the licence is separate**
* An **ISO** is a single file holding a whole installation disc
	* Attach to a VM = inserting a disc
	* Write to USB (e.g. Rufus, Media Creation Tool) = bootable on physical hardware

## Verifying a download (checksums)
* Vendors publish a **checksum** (hash) - a short fingerprint of the whole file
* Recalculate locally and compare; a mismatch = not the published file

```powershell
Get-FileHash .\Win11.iso -Algorithm SHA256
```
```bash
sha256sum ubuntu-24.04-desktop-amd64.iso
```

## Windows 11 hardware floor
* Supported **processor generation**, **4 GB RAM**, **64 GB storage**, **UEFI** firmware, **Secure Boot** capable, **TPM 2.0**
* The installer checks **before writing anything** and refuses if it fails
* Establish this **first** about any machine, not last

### TPM and Secure Boot - what they're for
* **TPM** - chip storing encryption keys in hardware; can't be copied off a drive like a file
* **Secure Boot** - checks boot code is **signed** before running it
* Both protect the machine **before the OS loads**

### When a machine doesn't qualify
* Some requirements are **settings** (fixable), others are **hardware facts** (not)
* Bypass tools exist - **this course doesn't use them**
* Unsupported installs **aren't entitled to updates**, including security updates

## Versions expire too
| Edition | Support per annual feature update |
| --- | --- |
| Home / Pro | **24 months** |
| Enterprise / Education | **36 months** |

* One feature update a year - installing is the *start* of a servicing cycle
