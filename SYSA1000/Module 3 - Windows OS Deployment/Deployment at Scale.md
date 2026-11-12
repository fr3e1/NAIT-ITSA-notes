---
course: SYSA1000
module: "Module 3 - Windows OS Deployment"
source: "SYSA1000 - Windows Deploy - Deployment at Scale.pptx"
tags: [SYSA1000, windows, deployment, sysprep, licensing]
---
# Deployment at Scale
Back to [[SYSA1000]] · Prev: [[Installing Windows]] · Next: [[User Accounts, Profiles, and Permissions]]

* A manual install takes most of an hour for **one** computer - no org installs 300 by hand

## Editions
| Edition | Key points |
| --- | --- |
| **Home** | Consumer. **Requires a Microsoft account** to finish setup |
| **Pro** | Adds business management: **BitLocker**, **domain join**, **Group Policy**, **Hyper-V** |
| **Enterprise / Education** | Pro + large-org features. **Not sold at retail** - volume agreements only |
| **LTSC** (Long-Term Servicing Channel) | Feature set frozen for years. Not "better" Windows - for machines that do **one job** (kiosks, medical, industrial) |

## Imaging
* Install once → configure → **capture** the installation to a single file (WIM) → apply to many machines
* Every machine starts identical

### Why a copied machine isn't a new machine
* Each install gets a **unique identifier (SID)** at setup; copying copies it
* Two machines with one identity confuse anything managing them (domains, WSUS, management tools)

### Sysprep makes a copy safe
* **Sysprep** strips the identity (SID, computer name, machine-specific config) before capture
* Each machine that receives the image runs first-boot setup (OOBE) again and generates its own identity

```cmd
C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown
```

## Answer files (unattended install)
* Setup asks the same questions every time; an **answer file** (`autounattend.xml` / `unattend.xml`) answers them in advance
* Same file → same result on every machine
* Made with **Windows System Image Manager** (Windows ADK), web generators, or AI - all produce **XML you are responsible for**

## Autopilot
* New machines arrive with Windows from the manufacturer; **Windows Autopilot** reshapes that install instead of replacing it
* Device is registered to the org before it's unboxed; the first user signs in and gets a configured machine

## Licensing
| Type | Bought by | Tied to |
| --- | --- | --- |
| **Retail** | A person | The person - can move to a new machine |
| **OEM** | Comes with a new computer | That computer, for life |
| **Volume** | An organization under an agreement | Managed centrally (KMS / MAK) |

> [!note] The **edition installed** and the **licence that activates it** are separate things
