---
course: SYSA1010
module: "Module 2 - Active Directory"
source: "Lesson 06a - User Management.pptx"
tags: [SYSA1010, active-directory, users, groups, ou]
---
# User Management
Back to [[SYSA1010]] · Prev: [[Identity Management]] · Next: [[Group Policy Objects]]

## Local vs domain user management
| | Local | Domain |
| --- | --- | --- |
| Model | Decentralized | Centralized |
| CLI | `net user` | `net user /domain` |
| PowerShell | `*-LocalUser` | `*-ADUser` |
| GUI | Settings, Local Users and Groups | **ADUC** |
| Organization | None | Containers + **OUs** |
| Deletes | Immediate | Immediate - unless **AD Recycle Bin** is enabled |

```powershell
New-ADUser -Name "Jordan Lee" -SamAccountName jlee -UserPrincipalName jlee@example.local `
  -Path "OU=Staff,DC=example,DC=local" -AccountPassword (Read-Host -AsSecureString) `
  -ChangePasswordAtLogon $true -Enabled $true
Disable-ADAccount jlee
Unlock-ADAccount jlee
Set-ADAccountPassword jlee -Reset
```

## Groups
* Domains can hold thousands of accounts → grant access to **groups**, not individuals (files, shares, apps, systems...)

### Group types
| Type | Use |
| --- | --- |
| **Security** | Grant access to resources |
| **Distribution** | Email lists only |
| **Mail-enabled security** | Security group with an email address |

### Group scopes
| Scope | Members from | Can be used in |
| --- | --- | --- |
| **Domain Local** | Anywhere | The **local domain only** (assign permissions here) |
| **Global** | **Local domain only** | Any domain in the forest |
| **Universal** | Any domain in the forest | Anywhere in the forest + across trusts |

> [!tip] Classic pattern: **AGDLP** - **A**ccounts → **G**lobal groups → **D**omain **L**ocal groups → **P**ermissions

* Everyone is in **Domain Users**; admin rights come from built-in groups (Domain Admins, Enterprise Admins...)

```powershell
New-ADGroup -Name "IT-Staff" -GroupScope Global -GroupCategory Security -Path "OU=Groups,DC=example,DC=local"
Add-ADGroupMember -Identity "IT-Staff" -Members jlee
Get-ADGroupMember "IT-Staff"
```

## OU structure
* Default locations: computers → **Computers** container, users → **Users** container (containers, not OUs - can't link GPOs)
* Design OUs from business requirements: **logical structure**, **geography**, or **access control** - **keep it simple**

### Accidental deletion protection
* On by default for new OUs
* To delete: ADUC → **View → Advanced Features** → OU Properties → **Object** tab → untick *Protect object from accidental deletion*
* Without the Recycle Bin, deletion is **immediate and permanent**

```powershell
New-ADOrganizationalUnit -Name "Staff" -Path "DC=example,DC=local" -ProtectedFromAccidentalDeletion $true
```

### AD Recycle Bin
* Enable in **Active Directory Administrative Center** (or PowerShell) - **can't be disabled once enabled**
* Deleted objects kept **180 days**, restorable to the same or a different location

```powershell
Enable-ADOptionalFeature 'Recycle Bin Feature' -Scope ForestOrConfigurationSet -Target example.local
Get-ADObject -Filter 'isDeleted -eq $true' -IncludeDeletedObjects | Restore-ADObject
```

## Best practices
* Simple OU structure, accidental protection on
* New users get **temporary passwords** (must change at logon) - a password known by more than one person is a risk
* Standard **naming scheme**
* **Don't reuse accounts**
* **Groups, not direct permissions**
* Admins get **two accounts**: a **privileged** one (e.g. Domain Admin) and a **standard** one for daily email/workstation use
* Role change → adjust group membership
* **Don't delete accounts** - **disable** them and remove group memberships

## References
* [Manage user accounts](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage-user-accounts-in-windows-server)
* [Understand security groups](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups)
* Exercise: *06a User Management*
