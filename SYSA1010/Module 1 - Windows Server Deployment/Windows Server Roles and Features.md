---
cssclasses: [sysa1010]
course: SYSA1010
module: "Module 1 - Windows Server Deployment"
source: "Lesson 02a - Windows Server Roles and Features.pptx"
tags: [SYSA1010, windows-server, roles, features]
---
# Windows Server Roles and Features
Back to [[SYSA1010]] · Prev: [[Introduction to Windows Server]] · Next: [[DHCP Server Role]]

* Modular components you install and configure; each one uses extra resources

| | Roles | Features |
| --- | --- | --- |
| Define | The server's **primary purpose** - services provided to the network/clients | **Extend functionality** / support roles (local or remote) |
| Examples | AD DS, DNS Server, DHCP Server, Web Server (IIS), Hyper-V | .NET Framework, Windows PowerShell, BitLocker, RSAT |
| Best practice | **Limit the number of roles per server** | Only what's needed |

## Ways to install
* **Server Manager → Manage → Add Roles and Features** (wait for the inventory to finish first)
* **PowerShell**
* **Windows Admin Center**
* **DISM**
* Same process on every version since Server 2012 (exercises use **Server 2025**, assignments start on **Server 2019**)

```powershell
Get-WindowsFeature                                  # list (Installed = [X])
Install-WindowsFeature DHCP -IncludeManagementTools
Install-WindowsFeature DNS, AD-Domain-Services -IncludeManagementTools
Uninstall-WindowsFeature Web-Server
```

## Best practices
* Only install the roles/features each server needs
* Each one increases resource use **and the attack surface**
* **Don't install Windows Admin Center on servers with active roles**

## References
* [Add or remove roles and features](https://learn.microsoft.com/en-us/windows-server/administration/server-manager/add-remove-roles-features)
* [Windows Admin Center](https://www.microsoft.com/en-us/windows-server/windows-admin-center)
