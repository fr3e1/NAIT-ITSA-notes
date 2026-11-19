---
cssclasses: [netw1111]
course: NETW1111
module: "Module 2 - Routing"
lesson: "M2 Day 4 - Subnets 2"
source: "Subnetting Considerations.pptx"
tags: [NETW1111, networking, subnetting, troubleshooting]
---
# Subnetting Considerations
Back to [[NETW1111]] · Prev: [[Subnetting]] · Next: [[Firewalls]]

Troubleshooting client/server misconfigurations in subnetted networks.

## Example 1 - class C-like subnets of a class A
```
Router 1: 10.0.1.1/24
Router 2: 10.0.2.1/24
Router 3: 10.0.3.1/24
```
* 16 bits stolen from `10.0.0.0/8` → each subnet defined by the **3rd octet**
* ✅ Easy boundaries, little confusion, 253 usable addresses (+ router) per subnet, add subnets by picking a new 3rd octet
* ⚠️ Could have just used multiple class C networks (`192.168.1.0`, `192.168.2.0`...)

## Example 2 - bigger subnets from a class B
```
Router 1: 172.20.128.1/20
Router 2: 172.20.192.1/20
Router 3: 172.20.224.1/20
```
* 4 bits stolen → up to **16 subnets** of 2^12 = **4096 addresses**
* Subnet = first 4 bits of the 3rd octet; host = last 4 bits of 3rd octet + all of the 4th
* Good for separate client/server/phone subnets or per-building subnets
* Larger subnets = more broadcast load on switches; easier but "messier"
* Smaller subnets = finer control and better security, but more management

## Example 3 - changing only the router
Start (works - all on one /24, clients talk directly, router is DG):
```
Router:  192.168.128.1/24
Client1: 192.168.128.101/24
Client2: 192.168.128.199/24
```
Change **only** the router to `/25` → splits into `.0/25` (bcast .127) and `.128/25` (bcast .255):
* Client1 ↔ Client2 still talk - both still think they're on one LAN
* Client1 → router ✅ - both see each other as same subnet
* Client2 → router: Client2 thinks the router is local and sends to it...
* ...but the router sees Client2 (.199) as **another network** → replies go to *its* DG → **replies fail**

> [!NOTE]
> Because everything is on the same switch/VLAN, the problem is purely **logical addressing**. With separate switches/VLANs, Client2 couldn't reach the router at all - which is one reason VLANs exist, though they add confusion when troubleshooting.

**Fix** - router gets an address in each subnet, clients' masks match their DG:
```
Router addr 1: 192.168.128.1/25
Router addr 2: 192.168.128.129/25
Client1: 192.168.128.101/25  (DG 192.168.128.1)
Client2: 192.168.128.199/25  (DG 192.168.128.129)
```
Now Client1 ↔ Client2 traffic goes through the router.

## Example 4 - spot the mistake
```
Router 1: 172.16.1.1/18
Router 2: 172.16.65.1/18
Client1:  172.16.63.255/18 (DG 172.16.1.1)
Client2:  172.16.64.255/18 (DG 172.16.65.1)
```
`/18` on a class B = 2 bits stolen → 4 subnets (first 2 bits of 3rd octet):

| Subnet bits | Range |
| --- | --- |
| 00 | 172.16.0.0 - 172.16.63.255 |
| 01 | 172.16.64.0 - 172.16.127.255 |
| 10 | 172.16.128.0 - 172.16.191.255 |
| 11 | 172.16.192.0 - 172.16.255.255 |

* Routers: `.1.1` in **00**, `.65.1` in **01** ✅
* **Client1** `63.255` = `(00)111111 11111111` → all host bits 1 → **broadcast address** ❌ - not usable
* **Client2** `64.255` = `(01)000000 11111111` → host bits not all 1s/0s → ✅ valid

> [!TIP]
> **Takeaway**
> A `.255` or `.0` octet doesn't automatically mean broadcast/network address - check the bits. But humans make mistakes, so avoid 0 and 255 in node addresses anyway.

## Summary - aim for
* Clean subnets that are easy to compute in your head
* Consider the impact of broadcast traffic
* Don't subnet unless there's a clear benefit
* Predictable, easy-to-spot default gateway addresses (e.g. `.1`)

Activity: *Sample Routing Quiz* - see [[Switching and Routing Lab]]
