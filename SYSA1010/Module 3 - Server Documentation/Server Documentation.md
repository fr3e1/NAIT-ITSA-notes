---
course: SYSA1010
module: "Module 3 - Server Documentation"
source: "Lesson 09a - Server Documentation.pptx"
tags: [SYSA1010, documentation]
---
# Server Documentation
Back to [[SYSA1010]] · Prev: [[Auditing Active Directory]]

## Why documentation matters
* Troubleshooting and **disaster recovery**
* **Knowledge transfer**
* Compliance and audits (+ an **audit trail**)
* Operational efficiency

## What to document
### Server and network
* Physical/virtual location
* Hostname and domain
* IP address(es), MAC address
* Custom firewall configuration
* Routing table changes

### Hardware
* CPU, memory
* Storage: disk configuration (**JBOD or RAID**), direct-attached or network
* NICs: single/multi-port, speed, connections

### Operating system
* Version and edition, **patch level**
* Server roles and their configuration
* Applications and services

### Break-glass account
* Emergency/disaster access account
* **Name and access** documented in normal documentation; **password stored securely** with minimal access

### Backup and recovery
* Backup software, **schedule**, **location**
* Recovery process for software/roles

### Role-specific
| AD DS | DHCP |
| --- | --- |
| Forest root domain | Scopes (network ID + mask) |
| Domains and child domains | Lease ranges |
| DCs in each domain | Exclusions |
| **FSMO role holders** (see [[Identity Management#FSMO roles]]) | Default gateway/router |
| External forest trusts | Other scope options |
| Sites | |
| Domain and forest functional level | |

### Diagrams
* Network diagrams (see [[Network Diagrams]]), server infrastructure, **rack layouts**, AD structure

## Documentation platforms
| Type | Examples |
| --- | --- |
| Knowledge base systems | Ticketing system KBs (Zendesk etc.) |
| Dedicated IT documentation | **IT Glue**, **IT Portal**, **ITFlow**, ConnectWise ITBoost |
| General knowledge platforms | Odoo Knowledge, wikis |
| Document repositories / collaborative | SharePoint, Git repos, **OneNote** |
| Decentralized | Individual files/notes (avoid) |
* **SOPs** (standard operating procedures) - step-by-step guides for common tasks

> [!tip] Something is better than nothing
> Start small and grow. **OneNote** is the recommended starting point - stored in Teams/SharePoint or anywhere central the team can reach.

## References
* [What is a knowledge base?](https://www.techtarget.com/whatis/definition/knowledge-base) · [What is an SOP?](https://www.techtarget.com/searchbusinessanalytics/definition/standard-operating-procedure-SOP)
* [IT documentation guide](https://betterdocs.co/it-documentation-guide-types-templates-best-practice/)
* [IT Glue](https://www.itglue.com/) · [IT Portal](https://www.itportal.com/) · [ITFlow](https://www.itflow.org/)
* [Working with local PSRepositories](https://learn.microsoft.com/en-us/powershell/gallery/how-to/working-with-local-psrepositories?view=powershellget-3.x)
* Exercises: *9a Research Documentation Platforms*, *9b SYSA1010 OneNote Template* - see [[Assignment 3 - Server Documentation]]
