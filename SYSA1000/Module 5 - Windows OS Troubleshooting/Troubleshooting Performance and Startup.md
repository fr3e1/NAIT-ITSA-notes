---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 5 - Windows OS Troubleshooting"
source: "SYSA1000 - Windows Troubleshoot - Troubleshooting Performance and Startup.pptx"
tags: [SYSA1000, windows, troubleshooting, startup, recovery]
---
# Troubleshooting Performance and Startup
Back to [[SYSA1000]] · Prev: [[Services, Processes, and Resource Monitoring]] · Next: [[Linux Distributions and the Ubuntu Installer]]

## A method, not a list of fixes
1. **Observe** what's actually happening (not what was reported)
2. **Narrow it down** to a layer before changing anything
3. **One hypothesis, one change**, then look again
4. **Record** everything

### Observe
* The report is someone's description; **the screen is the fact**
* Look at the corners, the clock, what does and doesn't work
* Compare against a known-good machine
* **Write it down before changing anything** - the first state can't be recovered

### Narrow it down
* Which layer: **boot, sign-in, service, network, program, setting**?
* Use tests that can only fail one way; order them from the machine outward (like the four network tests)
* The pattern of works/fails names the cause

### One change at a time
* Say what you think is wrong first → change **exactly one thing** → test the same way you observed
* Didn't help? **Put it back** - that's still a result

### Record
| Observed | Hypothesis | Change made | Result |
| --- | --- | --- | --- |
| | | | |
* The record is for the next person (often you). *A fix with no record is a guess that worked once*

## What runs when Windows starts
1. **Firmware** → boot loader reads the **BCD** (boot configuration store)
2. **Automatic services** start before sign-in
3. At sign-in: **startup apps** and **scheduled tasks** with a logon trigger
→ each layer has its own tool; the fault is usually in one layer

## Safe Mode
* Bare essential services and drivers only - no network, no startup apps, no scheduled tasks; "Safe Mode" in the corners
* Enter via **Startup Settings** in WinRE, or **msconfig → Boot → Safe boot**
* **Stuck in Safe Mode every boot** = the Safe boot flag is set → clear it in msconfig (or `bcdedit /deletevalue {current} safeboot`)

## Event logs are evidence
* **Windows Logs**: Application, Security, System - most answers in **Application** and **System**
* Each event: source, ID, level, time, message
* **Filter Current Log** by source/ID → 1000 lines become the 10 that matter
* Read for evidence of **what changed**

## Settings that lie
* *Set time automatically* only keeps the clock right if the **time zone** is right
* *Set time zone automatically* needs the location service (often off)
```cmd
tzutil /g                          :: current zone's internal name
tzutil /l                          :: list zones
tzutil /s "Mountain Standard Time"
```
* Alberta moved to **year-round UTC-6** in November 2026; an update added its zone

## A profile is a folder and a pointer
* Folder under `C:\Users\` **+** registry key pointing at it:
	`HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\<SID>` → `ProfileImagePath`
* If the folder doesn't exist → **temporary profile** + event **1511**
* Fix: make the folder name and `ProfileImagePath` match (change either one)

## When Windows won't start
* **WinRE** starts automatically after **two failed starts**
* From a working machine: **Shift + Restart**, or Settings → System → Recovery → Advanced startup
* Contains: **Startup Repair**, **Startup Settings** (Safe Mode), **Uninstall Updates**, **System Restore**, **Command Prompt**
* **Reset this PC is the last option**, not the first

See [[Assignment 5 - Windows OS Troubleshooting]]
