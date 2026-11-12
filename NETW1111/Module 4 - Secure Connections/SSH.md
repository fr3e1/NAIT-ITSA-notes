---
course: NETW1111
module: "Module 4 - Secure Connections"
lesson: "M4 Day 1 - Linux Remote Administration"
source: "SSH.pptx"
tags: [NETW1111, networking, ssh, linux, remote-admin]
---
# SSH
Back to [[NETW1111]] · Prev: [[Firewall Lab]] · Next: [[TLS]]

## Telnet → SSH
* Unix admins used **Telnet** (port 23) to log into servers remotely - **clear text**
* Attempts to replace it (rlogin, slogin) didn't take off; **SSH** (proposed **1995**) did
* **OpenSSH** released by the OpenBSD team in **1999** - most widely deployed and considered the most secure
* **SSH1 is deprecated**; SSH1 and SSH2 are incompatible - default is SSHv2

## What SSH does
* Encrypted, end-to-end, text-based client ↔ server communication
* Used heavily by Linux/BSD admins for quick manual tasks and **scheduled unattended tasks**
* **Port 22**, very NAT friendly
* Client and server **negotiate** authentication and encryption algorithms
* Includes an **SFTP subsystem**

## SSH keys (password-free login)
* Uses **asymmetric** cryptography - a **public** key (shared with servers) and a **private** key (kept on your client, guarded carefully)
* Data encrypted with one key can only be decrypted with the other

```bash
ssh-keygen                                       # 1. create key pair on the client (~/.ssh/id_ed25519 + .pub)
scp ~/.ssh/id_ed25519.pub user@server:~          # 2. copy the public key to the server
cat ~/id_ed25519.pub >> ~/.ssh/authorized_keys   # 3. (on server) authorize it
# or do 2+3 in one step:
ssh-copy-id user@server
ssh user@server                                  # now logs in without a password
```

## Clients
* **OpenSSH** (CLI + SFTP), **PuTTY** (CLI), **WinSCP** (SFTP only), **FileZilla** (SFTP only)

## SFTP and SCP
* **SFTP** - more secure than [[FTP]], more firewall friendly than FTPS; bound to SSH (needs SSH configured). **No anonymous SFTP**
* **SCP** ("secure cp") - non-interactive, good for scripting/one-off copies; works great with SSH keys for automation

```bash
sftp user@server         # interactive: ls, cd, get, put, quit
scp file.txt user@server:/tmp/
scp -r user@server:/var/log ./logs
```

## Windows and OpenSSH
* Microsoft resisted for decades, but now ships an **OpenSSH client and server** as installable features (a real port, fully compatible with Linux)
* Alternative to **WinRM** for remote scripting

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd; Set-Service sshd -StartupType Automatic
```

## SSH tunneling
* Run a weaker protocol through SSH to secure it
* Can also "sneak" blocked protocols (RDP, SMB) through if SSH is allowed
> [!warning] This may violate corporate policy - know the organization's stance first

```bash
ssh -L 3389:internal-host:3389 user@jumpbox   # local port forward RDP through SSH
```

## Links
* [RDP through SSH tunneling with PuTTY](https://www.cloudthat.com/resources/blog/a-guide-to-access-rdp-through-ssh-tunneling-using-putty)
* [sshd_config](https://www.ssh.com/academy/ssh/sshd_config) · [OpenSSH server config on Windows](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-server-configuration)
* Video: [Telnet vs SSH Explained](https://www.youtube.com/watch?v=tZop-zjYkrU)
* Activities: *Exercise - SSH*, *Exercise - Advanced SSH*
