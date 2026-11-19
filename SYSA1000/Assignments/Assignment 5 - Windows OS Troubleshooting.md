---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 5 - Windows OS Troubleshooting"
type: assignment
tags: [SYSA1000, assignment, troubleshooting, windows]
---
# Assignment 5 - Windows OS Troubleshooting
Back to [[SYSA1000]]

> [!NOTE]
> **Objective**
> Work a help desk ticket for "Morgan", whose Windows 11 VM has several faults. Diagnose each with the structured method, fix it with **one change at a time**, and record the reasoning.

## Setup
* Download `SYSA1000-A5-1261.zip` from **ITSA SharePoint → Course Materials → SYSA1000** (passwords are in the assignment doc)
* **Verify the hash** before extracting: `Get-FileHash SYSA1000-A5-1261.zip` vs the published SHA256
* It's a zipped folder (not an OVA): extract → **File → Open** the `.vmx` → enter the VM encryption password (vTPM ⇒ encrypted)
* Check the **Broken** snapshot exists and the adapter is **NAT (vmnet8)**

## The record (every part)
| What you observed | What you thought was causing it | What you changed | What happened |
| --- | --- | --- | --- |
* One row per change, in order. Failed attempts are rows too. See [[Troubleshooting Performance and Startup#A method, not a list of fixes]]

## Faults and fixes
### Part 1 - Boot: stuck in Safe Mode
* **Observe:** "Safe Mode" in the corners, black background, low resolution (no VMware drivers), no network
* **Cause:** **Safe boot (Minimal)** ticked in `msconfig` → Boot (= the `safeboot` value in the BCD)
* **Fix:** untick Safe boot (or `bcdedit /deletevalue {current} safeboot`) → restart. Network still broken (separate fault)
```cmd
bcdedit /enum
bcdedit /deletevalue {current} safeboot
```

### Part 2 - Time: an hour behind since Nov 1
* **Observe:** *Set time automatically* = On, *Set time zone automatically* = Off, zone = **(UTC-07:00) Mountain Time (US & Canada)**
* **Cause:** the **time zone** is wrong, not the time. Alberta moved to **year-round UTC-6** on Nov 1 2026; the US/Canada Mountain zone still fell back to UTC-7
* **Fix:** set **(UTC-06:00) Mountain Time (Alberta)** - ID `Alberta Standard Time`
```powershell
tzutil /g              # Get-TimeZone
tzutil /l              # Get-TimeZone -ListAvailable
tzutil /s "Alberta Standard Time"   # Set-TimeZone -Id "Alberta Standard Time"
```

### Part 3 - Network: nothing reachable (two layers)
| Layer | Observation | Cause | Fix |
| --- | --- | --- | --- |
| Addressing | `169.254.x.x`, *DHCP Enabled: No*, no gateway | **DHCP Client** service **Stopped / Disabled**; System log **event 7040** shows when it was changed | Startup type → **Automatic**, start it, `ipconfig /renew` |
| Name resolution | Gateway + `8.8.8.8` OK, `dns.google` fails, nslookup times out asking a bogus server | DNS server **hard-coded** in IPv4 properties (a bad "speed tip" from an AI chatbot) | *Obtain DNS server address automatically* |

```powershell
Get-Service Dhcp; sc qc Dhcp
Get-WinEvent -FilterHashtable @{LogName='System'; Id=7040}
Set-Service Dhcp -StartupType Automatic; Start-Service Dhcp; ipconfig /renew
Get-DnsClientServerAddress -AddressFamily IPv4
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ResetServerAddresses
Resolve-DnsName nait.ca
```

### Part 4 - Performance: slow, disk full, no Task Manager
| Fault | Found with | Fix |
| --- | --- | --- |
| "Task Manager has been disabled by your administrator" | `gpedit.msc` → User Config → Admin Templates → System → Ctrl+Alt+Del Options → **Remove Task Manager = Enabled** (`DisableTaskMgr=1` under `HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System`) | Set to **Not Configured** |
| High CPU | `resmon` / `tasklist /v` / Process Explorer → hidden `powershell.exe ... -File C:\ProgramData\...\maintenance.ps1` | **Disable the scheduled task** that starts it (*At log on* trigger), **then** end the running process |
| Disk nearly full | Settings → Storage / largest-files listing → a huge hidden `.cache` file in the same folder as the script | Delete it |

```powershell
Get-CimInstance Win32_Process | Select Name, ProcessId, CommandLine
Get-ScheduledTask | Where State -ne Disabled
Stop-Process -Id 1234
Get-ChildItem C:\ -Recurse -Force -File -ErrorAction SilentlyContinue |
  Sort Length -Descending | Select -First 10 FullName, Length
```
> [!TIP]
> Disabling a task stops it starting again but doesn't stop the copy already running - and killing the process alone doesn't stop it coming back.

### Part 5 - Profile: temporary profile after a name change
* **Observe:** signing in shows *"We can't sign in to your account"* + empty desktop (temp profile). Application log: **User Profile Service event 1502/1511**
* **Cause:** a technician renamed `C:\Users\<old>` → `C:\Users\<new>` but **not** `ProfileImagePath` under `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\<SID>`
* **Two valid fixes:** rename the folder back, **or** update `ProfileImagePath` to the new folder - pick one and justify it
* ⚠️ The common online fix (delete the `.bak` key) is for a **different cause** - look at *this* machine's registry first
```powershell
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList' |
  Get-ItemProperty | Select PSChildName, ProfileImagePath
```

> [!NOTE]
> Restoring the snapshot to retry is fine - say so in the record.

Related: [[Services, Processes, and Resource Monitoring]], [[Troubleshooting Performance and Startup]], [[Network Addressing and Name Resolution]]
