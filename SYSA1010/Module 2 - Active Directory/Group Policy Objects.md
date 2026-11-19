---
cssclasses: [sysa1010]
course: SYSA1010
module: "Module 2 - Active Directory"
source: "Lesson 07a/07b - Group Policy Objects, Part 1 and 2.pptx"
tags: [SYSA1010, active-directory, group-policy, gpo]
---
# Group Policy Objects
Back to [[SYSA1010]] · Prev: [[User Management]] · Next: [[Windows Event Logs]]

## What is policy?
* Rules defining **user experience** and/or **system behaviour**
* Enforce standards/consistency, improve security and reliability
* Managed **locally** or **centrally**, applied **automatically**; need local/domain admin to manage

## Local policy
* `gpedit.msc` - Local Group Policy Editor (**Pro edition or higher**)
* `secpol.msc` - Local Security Policy (some settings overlap/conflict with gpedit)
* Processed **first**, so domain GPOs can overwrite it
* Inconsistent in large environments

## Group Policy Objects (GPOs)
* Collections of settings for computers and users in a **domain**, stored in **AD**, linked to **sites, domains or OUs**
* Centralize security settings, software deployment, user experience

| Section | Applies to | When |
| --- | --- | --- |
| **Computer Configuration** | The computer, regardless of user | At **startup** |
| **User Configuration** | The user, regardless of workstation | At **logon** |

* **Policies** - enforced, user can't change
* **Preferences** - defaults the user can change

### Refresh
* Initial GPOs must apply within 60 minutes
* Background refresh every **90 minutes + random 0-30 min** (staggered to protect DCs; adjustable by GPO)
* Some changes need a **reboot** (or logoff)

```powershell
gpupdate /force                 # /target:user or /target:computer
gpresult /r                     # what applied to me
gpresult /h report.html
```

## GPMC and processing order
* **Group Policy Management Console** - on DCs (or RSAT); manages GPOs stored in AD
* Order: **L**ocal → **S**ite → **D**omain → **OU** (**LSDOU**) - **last applied wins** on conflicts
* Domain-linked GPOs are **inherited** by OUs and sub-OUs
* **Block Inheritance** on an OU → only GPOs linked to that OU apply (*Enforced* links override blocking)

## Default policies
* **Default Domain Policy** - security settings for all computers; holds the **initial password policy**
* **Default Domain Controllers Policy** - security settings for DCs only
* Modifying defaults is usually **against best practice**, except: the **password policy** (if not using Fine-Grained Password Policies) and **DC security settings**

### Password policy path
```
Default Domain Policy → Computer Configuration → Policies → Windows Settings →
  Security Settings → Account Policies → Password Policy
```

## Filtering
* **Security filtering** - which users/groups/computers the GPO applies to
* **WMI filters** - SQL-like queries (e.g. only Windows 11 24H2); **one WMI filter per GPO**
* **Item-level targeting** - in Preferences, target by user/computer properties

## Creating new GPOs (best practice)
1. Create under **Group Policy Objects** with a concise name
2. Configure **either computer or user** settings - not both
3. **Link to a test OU** first
4. Verify it applies correctly
5. Link to the production OU

```powershell
New-GPO -Name "Workstation - Power Settings" | New-GPLink -Target "OU=Workstations,DC=example,DC=local"
Get-GPOReport -Name "Default Domain Policy" -ReportType Html -Path .\ddp.html
```

## Example GPOs
| Setting | Path |
| --- | --- |
| Password policy | Computer Config → Policies → Windows Settings → Security Settings → Account Policies → Password Policy |
| Windows Update | Computer Config → Administrative Templates → Windows Components → Windows Update |
| Power/sleep | Computer Config → Administrative Templates → System → Power Management |

Other examples: deploy software, folder redirection, browser config, removable storage control, firewall rules, Start menu, block Control Panel/Settings, logon messages, Windows Hello, default file associations, Defender, Store, wallpaper, mapped drives, printers.

## Storage
* Every DC holds a copy in the **SYSVOL** share - `C:\Windows\SYSVOL`
* Viewing settings in GPMC may be blocked by **IE Enhanced Security Configuration** - disable it for admins or add `about:security_mmc.exe` to trusted sites

## References
* [Group Policy overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview) · [Scope](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-scope) · [Processing](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-processing)
* [WMI filter cheat sheet](https://www.dannymoran.com/wmi-filter-cheat-sheet/)
* Exercises: *07a Modify the default Password Policy*, *07b Create new GPOs*
