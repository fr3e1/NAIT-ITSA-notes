---
cssclasses: [sysa1010]
course: SYSA1010
module: "Module 2 - Active Directory"
source: "Lesson 08b - Auditing Active Directory.pptx"
tags: [SYSA1010, active-directory, auditing, event-logs]
---
# Auditing Active Directory
Back to [[SYSA1010]] · Prev: [[Windows Event Logs]] · Next: [[Server Documentation]]

## Custom views
* Reusable filters on specific event IDs/sources across **one or more logs** (no need to redo *Filter Current Log*)
* Workstations ship with 1 custom view (Administrative Events); servers gain more as roles are installed
* *Event Viewer → Custom Views → Create Custom View* - same options as filtering, but multiple logs/sources

## AD auditing
* **On by default** in AD DS on Server 2019/2022/2025; older domains may need it enabled by **GPO on the DCs**
* Dozens of AD-related event IDs (~147)

### Key Security log event IDs
| Event ID | Meaning |
| --- | --- |
| **4624** | User logon |
| **4625** | Failed logon |
| **4634** | User logoff |
| **4720** | User account created |
| **4722** | User account enabled |
| **4723** | User changed own password |
| **4724** | Password reset (by admin) |
| **4725** | User account disabled |
| **4726** | User account deleted |
| **4740** | Account locked out |
| **4767** | Account unlocked |
| **4727** | Global security group created |
| **4728** | Member added to global security group |
| **4729** | Member removed from global security group |
| **4730** | Global security group deleted |

## Analyzing a scenario - locked-out user back from vacation
1. Help desk **verifies identity**
2. **Resets the password** (4724)
3. **Unlocks** the account (4767)
4. Sets **change password at next logon**
5. Relays the password **securely**
6. User logs in (4624) and changes it (4723)

### How to read it
* Event Viewer lists **newest first** → filter by the IDs, then sort ascending by date
* Interpret in **chronological order** - one event rarely tells the story; a **series** gives context
* **Export** and attach to the ticket/incident

## Exporting
* Event Viewer: *Save All Events As* / *Save Filtered Log File As* (whole log, filtered view or custom view) → `.evtx`
* **Include display information** (bigger file, but readable on systems without the event definitions) - recommended
* Saved logs open in Event Viewer on any Windows machine (*Open Saved Log*)

```powershell
wevtutil epl Security C:\Logs\security.evtx
wevtutil qe Security /q:"*[System[(EventID=4724)]]" /f:text /c:10
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4740,4767,4724} | Sort TimeCreated
```

## References
* [Audit logon events](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/basic-audit-logon-events)
* [Windows event IDs for Active Directory](https://infrasos.com/complete-list-of-windows-event-ids-for-active-directory/)
* [wevtutil](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil)
* Exercise: *08b Audit Active Directory Events* - see [[Assignment 2 - Active Directory Configuration and Auditing]]
