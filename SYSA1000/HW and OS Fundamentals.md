# Firmware - The software before the operating system
* Firmware is software stored on the hardware itself
* it runs before any operating system exists on the machine
* It brings the hardware up and hands over to te OS 
* On a PC this firmware is BIOS or UEFI

## BIOS
### The Original Standard
* Basic input/output system, dating from 1975
* text only interface, keyboard driven
* limited to 1MB of memory
* cannot boot from drives larger than 2TB

## UEFI
### What replaced it
* Unified Extensive Firmware Interface, introduced 2005
* Graphical interface, mouse supported
* Boots faster and supports drives over 2TB
* Adds security features BIOS could not

## POST and the Boot Sequence
* POST is the Power-On Self-Test
* Firmware checks the hardware before anything else runs
* A failure here stops the machine before any OS loads
* If POST passes, firmware hands off to the boot manager

## Boot Order and the Boot Manager
* Firmware holds an ordered list of bootable devices
* It tries each in turn until one works
* The boot manager then chooses which OS to start
* Changing boot order is how you boot from a USB installer

## Secure Boot
* A firmware feature that checks what it is about to load
* Only software signed by a trusted vendor is allowed to boot
* Blocks bootkits that would otherwise load before the OS 
* Can be turned off, and sometimes has to be

## TPM 2.0 and Windows 11
* TPM is the Trusted Platform Module, a security chip
* It stores encryption keys and measures the boot state
* Windows 11 requires TPM 2.0 and Secure Bot
* Disabling a TPM and clearing a TPM are very different actions

## Firmware power and compatibility settings
* Firmware controls what the machine does with power before any OS 
* Wake on LAN, and behaviour after a power cut
* Fan and thermal profiles
* virtualization support, which Module 2 depends on