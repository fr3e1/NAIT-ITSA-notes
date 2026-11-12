---
course: NETW1111
module: "Module 4 - Secure Connections"
lesson: "M4 Day 3 - Wireless Theory"
source: "Wireless.pptx"
tags: [NETW1111, networking, wireless, wifi, security]
---
# Wireless Networking
Back to [[NETW1111]] · Prev: [[TLS]] · Next: [[Wireless Assessment]]

* Wired is the gold standard for speed and reliability, but isn't always practical or affordable
* Wireless filled gaps (rural satellite, point-to-point antennas), then went mainstream (homes, airports, cafes, schools)

## Standards
* All Wi-Fi derives from **IEEE 802.11**; speeds increase each generation
* NICs only support a subset of versions
* Main frequencies: **2.4 GHz** and **5 GHz** (6 GHz on Wi-Fi 6E/7)

| Standard | Wi-Fi name | Bands |
| --- | --- | --- |
| 802.11b/g | - | 2.4 GHz |
| 802.11a | - | 5 GHz |
| 802.11n | Wi-Fi 4 | 2.4 / 5 GHz |
| 802.11ac | Wi-Fi 5 | 5 GHz |
| 802.11ax | Wi-Fi 6 / 6E | 2.4 / 5 (/ 6) GHz |
| 802.11be | Wi-Fi 7 | 2.4 / 5 / 6 GHz |

## Devices
* **Wireless router** - separate wireless network/LAN, routed to the wired network/Internet via **NAT**
* **Wireless access point (AP)** - extends the wired LAN directly (same DHCP, DNS etc.)
* Clients need a wireless NIC (or USB adapter) supporting a common 802.11 standard

## Benefits vs drawbacks
| ✅ Benefits | ❌ Drawbacks |
| --- | --- |
| Cheaper/easier to set up | Slower than wired (theoretical and real) |
| No cabling through old buildings | Too many clients → degraded performance |
| Connect from anywhere, phone friendly | Dead zones; walls, pillars, furniture block signal |
| Multiple WLANs (guest vs staff) on different bands | Interference from other devices |
| Easy integration with wired | Signal falls off with distance |

## Interference
* Wi-Fi uses the unlicensed **ISM bands** (industrial, scientific, medical)
* **2.4 GHz** competitors: microwaves, cordless phones, Bluetooth, smart home devices
	* Longer wavelength → **longer range**, better through walls/floors
* **5 GHz** - fewer non-IT devices → less interference; **faster** at closer range with fewer obstructions
* The best config depends on the environment

## Bandwidth
* Wireless is a **broadcast** technology → behaves like a **hub**, not a switch
* Bandwidth is **shared** - more clients = smaller share each

### MU-MIMO
* Up to 802.11n: **SU-MIMO** - one client transmits at a time (hub-like)
* 802.11ac: **MU-MIMO** - up to 4 simultaneous downloads on **5 GHz**
* 802.11ax: 8+ streams, including 2.4 GHz

## Security
* Everyone in range can snoop broadcasts → encryption needed

| Protocol | Year | Details |
| --- | --- | --- |
| WEP | 1997 | Wired Equivalent Privacy. Cracked 2001, **don't use** |
| WPA | 2003 | Interim subset of 802.11i (TKIP) |
| **WPA2** | 2004 | Full 802.11i, **AES**. Pre-shared key (**Personal**) or auth server (**Enterprise**/RADIUS). Recommended baseline |
| **WPA3** | 2018 | Based on WPA2, Wi-Fi Alliance certified equipment |
| WPS | 2006 | PIN/push-button setup. PIN crackable (2011) → reveals the PSK. **Disable it** |

> [!warning] The PSK only keeps *outsiders* out
> Anyone who knows the pre-shared key can snoop traffic on the network - like an apartment parkade that keeps strangers out but not a renter doing a hit-and-run. Always use **TLS** on wireless networks as an extra layer.

## Mesh vs access points
* **Mesh** - boosters that decentralize routing to eliminate dead zones and extend range; but node-to-node traffic **eats shared wireless bandwidth**
* **Wired APs** - backhaul over the wired network, so all wireless bandwidth goes to clients. Placement takes testing: avoid too much overlap/interference while allowing seamless roaming

## 5G ≠ 5 GHz
* **5G** = cellular network technology (low/mid/high bands), long range, more power
* **5 GHz** = a Wi-Fi band
* 5G modules/routers exist for remote sites without wired Internet

## Troubleshooting checklist
1. Can the client **see and connect to the SSID**?
2. Does it get a **valid IP**?
3. Does it get **router and DNS** settings - can you ping them?
4. Any **obstructions** (walls etc.)?
5. Any **competing devices** (microwaves, baby monitors, other routers)?
6. Is the client **802.11-compatible** with the router/AP?

## Links
* [IEEE 802.11](https://en.wikipedia.org/wiki/IEEE_802.11) · [WPA](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Access) · [WPS](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Setup) · [ISM band](https://en.wikipedia.org/wiki/ISM_radio_band) · [Mesh](https://en.wikipedia.org/wiki/Wireless_mesh_network)
* Videos: [Mesh WiFi Explained](https://www.youtube.com/watch?v=XaOcjHrdMFE) · [2.4 GHz vs 5 GHz WiFi](https://www.youtube.com/watch?v=J_bf_KE5llQ)
* Activity: *Exercise - Wireless*
