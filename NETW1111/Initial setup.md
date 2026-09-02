# Installing VMWare (no login needed)
 1. Visit [ITSA Sharepoint](https://naitca.sharepoint.com/sites/ITSAProgram) website
 2. visit Course Materials
 3. Find required .exe at "General Downloads/software/"

![VMWare exe location](/images/VMWare_exe_location.png)


# Setting up VMWare Network configs
**Note: info gathered through VMWare network setup video in brightspace**
## Accessing network settings
### On top bar:
* Edit > Virtual Network Editor

**Make sure to enable elevated privileges to edit the settings**
 * On Linux, a prompt will pop up to give sudo privs
## Changing default vmnet settings
### vmnet1
* Change subnet IP to 192.168.201.0
### vmnet8
* Change subnet IP to 192.168.202.0
* leave network type to NAT
## Adding a new vmnet
* Add network, network name should be "vmnet4"
* set to Host-only
* set subnet IP to 192.168.204.0
* Disable DHCP

## Example finished config
![Example finished config](/images/Network_config_example.png)
## In case of major issues
Simply restore defaults and redo every changes noted above
