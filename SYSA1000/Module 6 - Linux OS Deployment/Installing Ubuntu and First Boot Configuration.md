---
course: SYSA1000
module: "Module 6 - Linux OS Deployment"
source: "SYSA1000 - Linux Deploy - Installing Ubuntu and First Boot Configuration.pptx"
tags: [SYSA1000, linux, ubuntu, terminal, apt]
---
# Installing Ubuntu and First Boot Configuration
Back to [[SYSA1000]] · Prev: [[Linux Distributions and the Ubuntu Installer]]

## Why a terminal?
* Text in, text out - output can be read, saved, searched, compared
* Works with no screen, over a network, or in a script
* GUI tools change between releases; the commands underneath rarely do

## Reading the prompt
```
user@hostname:~$
```
| Part | Meaning | Command |
| --- | --- | --- |
| `user` | Who you are | `whoami` |
| `hostname` | Machine name | `hostname` |
| `~` | Where you are (`~` = home) | `pwd` |
| `$` / `#` | Ordinary user / **root** | |

## sudo
* "superuser do" / "substitute user do" - both right
* Ubuntu has **no root login**; the first account runs single commands as root with `sudo`
* Asks for **your own** password (shows nothing as you type); remembered **15 minutes** per terminal
* Changing the system needs sudo; only reading it doesn't

## Package managers
* **Package** = program + what it needs, one file with a name and version
* **Dependency** = another package it needs first
* The package manager installs/removes/updates, resolves dependencies, fetches from **repositories**, **checks signatures**, tracks what's installed

### apt (Advanced Package Tool)
```bash
sudo apt update            # refresh the list of what repos offer - installs nothing
sudo apt upgrade           # install newer versions of installed packages
sudo apt install htop
sudo apt remove htop
apt list --installed
```
* "All packages are up to date" when done; **phased updates** may show as *kept back* - normal

### snap
* Some apps (e.g. **Firefox**) are **snaps** - dependencies bundled inside
* `snapd` checks for updates **4×/day** on its own
```bash
sudo snap refresh          # "All snaps up to date"
snap list
```
> [!important] apt and snap don't see each other - "fully up to date" means **both**

## cat, grep and pipes
```bash
cat /etc/hostname                         # machine's name
cat /etc/os-release                       # release details
less /var/log/syslog                      # long files (q to quit)
grep AutomaticLogin /etc/gdm3/custom.conf # only matching lines
grep -i -n error file.txt                 # -i ignore case, -n line numbers, -v invert
cat /etc/gdm3/custom.conf | grep AutomaticLogin
lsblk -f | grep -v loop                   # drop loop devices - where pipes shine
```
* `cat` = con**cat**enate (join files end to end and print)
* `grep` = from ed's `g/re/p` - **g**lobal **r**egular **e**xpression **p**rint
* `|` (pipe) sends one command's output into the next - commands are built to be joined

## Getting help
```bash
grep --help
man grep         # q to leave
man -k network   # search manuals by keyword
```
* Man sections: **1** user commands, **5** file formats, **8** admin commands

## Reading the machine
```bash
lsb_release -a        # distro + release
nproc                 # CPU cores available
free -h               # memory (total < what the VM was given - kernel reserves some)
lsblk -f /dev/sda     # partitions, FS, mount points
timedatectl           # time zone + clock sync status
ip -br addr           # interfaces, state, addresses - one line each
ping -c 4 dns.google  # 4 echo requests (Linux ping runs forever without -c)
```
* In config files, a line starting with `#` is a **comment** - not in effect

## Snapshot
* Take a snapshot of the **updated** install and verify it restores (see [[Virtual Machine Operations#Snapshots]])

## Exercise
* *Secure Boot and Disk Encryption on Ubuntu Desktop*
