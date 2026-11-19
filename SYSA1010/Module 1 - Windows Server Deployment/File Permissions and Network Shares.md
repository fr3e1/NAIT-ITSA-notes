---
cssclasses: [sysa1010]
course: SYSA1010
module: "Module 1 - Windows Server Deployment"
source: "Lesson 04a - File Permissions and Network Shares.pptx"
tags: [SYSA1010, windows-server, ntfs, shares, permissions]
---
# File Permissions and Network Shares
Back to [[SYSA1010]] · Prev: [[DNS Server Role]] · Next: [[Identity Management]]

* Two layers of protection:
	* **Local** - file and folder (NTFS) permissions
	* **Network** - share permissions
	* Businesses **combine both** for granular control

## Sharing a computer safely
* Every user has their own account with a unique password
* Lock folders down to just the people who need them
* Limit who has **local administrator** (needed to manage local accounts)

## Local users
* `lusrmgr.msc` / Computer Management → Local Users and Groups
* Attributes: user name, full name, description, password + password options, disabled, locked out, group membership

## File and folder (NTFS) permissions
* **Security** tab = the **ACL** (Access Control List) - NTFS and ReFS
* Basic: **Read**, **List Folder Contents**, **Read & Execute**, **Write**, **Modify**, **Full Control**
* **Allow** or **Deny** - Deny overrides Allow
* The **owner** can always modify permissions
* **Advanced** (recommended): view/change owner, scope to *this folder / files / subfolders*, enable/disable **inheritance**, granular permissions

### Effective NTFS permissions
* Explicit **Deny overrides** any Allow
* Otherwise permissions from all your groups **add up** - the **most permissive** wins

## Network shares
* Share a folder over the network - share permissions apply **only over the network**, separate from NTFS
* Requires **File and Printer Sharing**; **Network Discovery** helps find computers in workgroups
* Share permissions: **Read**, **Change**, **Full Control**

```powershell
New-SmbShare -Name "Data" -Path "D:\Data" -FullAccess "Administrators" -ChangeAccess "Staff" -ReadAccess "Everyone"
Get-SmbShareAccess -Name "Data"
Grant-SmbShareAccess -Name "Data" -AccountName "Students" -AccessRight Read -Force
```

## Combined effective permissions (over the network)
1. Work out the **most permissive share** permission
2. Work out the **most permissive NTFS** permission
3. The user gets the **least permissive of the two**
4. Any **Deny** in either → denied

| Share | NTFS | Effective |
| --- | --- | --- |
| Full | Full | **Full** |
| Full | Modify | **Modify** |
| Read | Change (Modify) | **Read** |
| Change | Deny Read | **Denied** |
| Deny Full | Read | **Denied** |

> [!TIP]
> Common practice: share = *Everyone/Authenticated Users: Full* (or Change) and control access with NTFS only.

Related: [[User Accounts, Profiles, and Permissions]] (SYSA1000), [[SMB]] (NETW1111)
* Exercise: *04a Create Network Share*
