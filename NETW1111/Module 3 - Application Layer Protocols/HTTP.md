---
cssclasses: [netw1111]
course: NETW1111
module: "Module 3 - Application Layer Protocols"
lesson: "M3 Day 3 - HTTP Protocol"
source: "HTTP.pptx"
tags: [NETW1111, networking, http, web]
---
# HTTP
Back to [[NETW1111]] · Prev: [[Packet Sniffing]] · Next: [[FTP]]

* **HyperText Transfer Protocol** - a primary application protocol of the web
* Proposed by **Tim Berners-Lee** in 1989
* Client/server **request/response**: client connects → server returns resource (or error) → connection ends
* **TCP port 80**, assumes a reliable connection

## URLs
* **Uniform Resource Locator** - specifies the resource requested
* `scheme://host:port/path?query#fragment` e.g. `http://www.example.com:80/docs/index.html?id=5#top`
* Client parses the URL, connects to the server, requests the resource

## Methods
| Method | Purpose |
| --- | --- |
| **GET** | Request a resource (the main method) |
| **POST** | Send data that modifies content (forms, logins, DB updates) |
| **HEAD** | Like GET but only info *about* the resource |
| PUT | Replace a resource (rarely enabled) |
| DELETE | Remove a resource (rarely enabled) |
| TRACE | Proxies declare themselves in the header to trace the path |
| OPTIONS | Ask what the server supports |
| CONNECT | SSL connections through proxies |

## Response codes
| Range | Meaning |
| --- | --- |
| 1xx | Informational |
| 2xx | Success |
| 3xx | Redirect - client needs to do more |
| 4xx | Client request problem |
| 5xx | Server problem |

| Code | Meaning |
| --- | --- |
| 200 OK | Found and returned |
| 301 Moved | Moved permanently to a new URL |
| 400 Bad Request | Request is malformed |
| 404 Not Found | Not on this server |
| 500 Internal Server Error | Application crashed on the server |
| 503 Unavailable | Temporarily unavailable |

## Versions
| Version | Year | Key change |
| --- | --- | --- |
| 0.9 / 1.0 | 1991 / 1996 | New TCP connection per resource (images loaded after pages) |
| 1.1 | 1997 | **Persistent connections**, streaming, pipelined requests |
| 2.0 | 2015 | Speed/efficiency; browsers only implement the encrypted version; ~35% adoption (2025) |
| 3.0 | draft | Runs over **UDP** (QUIC); ~95% browser support, ~35% of servers (Google, Cloudflare) |

## State
* HTTP is **stateless** - server forgets previous requests once the TCP connection closes
* Simulated with **cookies** (stored on the client - can be modified by a malicious client) and **databases** (burden on the server) - usually both together

## Authentication
* HTTP supports usernames/passwords but rarely used; passing them in the URL isn't secure
* Usually handled by the website: SSL for encryption + backend DB for credentials
* Apache uses `.htaccess`; IIS integrates with Active Directory

## Web servers
| Product | Description |
| --- | --- |
| Apache | Open source, usually Linux; the A in **LAMP** (Linux, Apache, MySQL, PHP) |
| IIS | Microsoft; SharePoint, C# sites; integrates with AD |
| Squid | Caching proxy |
| Apache Tomcat | Java server pages |
| Nginx | Lightweight open source alternative to Apache |

## Proxies and caching
* **Web proxy** - middleman; sanitizes malicious requests to protect the real server (an attack may take down only the proxy)
* **Reverse proxy** - can provide anonymity to clients
* **Caching server** - stores responses for a set time → less bandwidth, faster replies

## Command line clients
```bash
telnet
> o www.google.com 80
GET / HTTP/1.1
Host: www.google.com

wget http://www.google.com/ -O googlewget.html
curl -L http://www.google.com -o googlecurl.html
curl -I http://www.google.com     # HEAD request - headers only
```
* Browser **developer tools** (F12) show raw requests/responses

## Firewalls and encryption
* Very firewall friendly - just allow TCP 80 in. So friendly that apps (e.g. Skype) use 80/443 to bypass restrictions
* HTTP has poor native encryption → run it inside **SSL/TLS** tunnels (**HTTPS, 443**) - see [[TLS]]
* TLS is slower and uses more server resources, but privacy concerns pushed the web to encrypt everything

## Links
* [HTTP](https://en.wikipedia.org/wiki/HTTP) · [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2) · [HTTP/3](https://en.wikipedia.org/wiki/HTTP/3)
* Activity: *Exercise - HTTP*
