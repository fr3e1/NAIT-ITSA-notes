---
cssclasses: [sysa1000]
course: SYSA1000
module: "Module 4 - Windows OS Configuration"
source: "SYSA1000 - Windows Config - User Accounts, Profiles, and Permissions.pptx"
tags: [SYSA1000, windows, accounts, ntfs, permissions]
---
# User Accounts, Profiles, and Permissions
Back to [[SYSA1000]] · Prev: [[Deployment at Scale]] · Next: [[Command Line, Firewall, and System Settings]]

## Accounts
* Every action on Windows is done by an **account**
* **Local account** - exists on this one machine only (used in this course)
* **Microsoft account** - email identity that follows you between devices

### Standard user vs administrator
* **Standard user** - can use the machine but not change it
* **Administrator** - can change anything after a **UAC consent prompt**
* Day-to-day work belongs on a standard account, even for admins

### Groups
* A named collection of accounts; built-ins that matter: **Users** and **Administrators**
* Real world: grant permissions to **groups**, add accounts to groups (this course grants to accounts directly so you can see the mechanism)

```powershell
New-LocalUser -Name "jdoe" -Password (Read-Host -AsSecureString)
Add-LocalGroupMember -Group "Administrators" -Member "jdoe"
Get-LocalGroupMember Users
net user jdoe /active:no
lusrmgr.msc       # Local Users and Groups (Pro+)
```

## Profiles
* Profile = the account's own folder tree + settings, under `C:\Users\<name>`
* **Creating an account doesn't create the profile** - the **first sign-in** does

## Security identifiers (SIDs)
* Windows tracks every account by a **SID** (e.g. `S-1-5-21-...-1001`), issued once and never reused
* Permissions are stored against the SID → **renaming changes nothing**
* Deleting an account leaves its SID behind as an unknown entry on folders

```powershell
whoami /user
Get-LocalUser | Select Name, SID
```

## NTFS permissions
* Every file/folder has an **ACL** - entries naming an account/group and its permissions
* Six basic permissions: **Full control**, **Modify**, **Read & execute**, **List folder contents**, **Read**, **Write**
* **Full control** is the one nobody needs by default (it can change permissions/take ownership)

### Inheritance
* New folders inherit their parent's permissions (shown **greyed out**, can't edit in place)
* `C:\` root grants **Users** read and **Authenticated Users** modify below it - every account you create is in both

### Locking a folder down
1. **Disable inheritance** → *Remove all inherited permissions*
2. The list is now empty - nobody (not even you) can open it
3. **Add** the intended entries by name
4. Only then **Apply** - saving an empty list locks everyone out

### Allow, Deny, no entry
* **No entry** → refused (implicit deny)
* **Deny** beats Allow **at the same level**
* An **explicit** entry beats an **inherited** one - even an inherited Deny
* Admins prefer removing/granting; keep Deny for rare cases

## Effective access
* What an account can actually do = the sum of every applicable entry
* **Advanced → Effective Access** tab calculates it for one account - a **prediction**
* **Signing in as that user is the proof** - testing as an admin proves nothing

```powershell
icacls C:\Data
icacls C:\Data /inheritance:r
icacls C:\Data /grant "jdoe:(OI)(CI)M"
```

See [[Assignment 4 - Windows OS Configuration]] · Related: [[File Permissions and Network Shares]] (SYSA1010)
