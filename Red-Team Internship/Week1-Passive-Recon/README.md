# Week 1 — Passive OSINT & Subdomain Enumeration

**Internship:**
**Target:** hackthissite.org (authorized scope)  
**Type:** Passive Reconnaissance — zero direct interaction with target

---

## Objective
Map the external attack surface of the target domain using 
passive OSINT techniques only. No active scanning performed.

---

## Tools Used
| Tool | Purpose |
|------|---------|
| Subfinder | Passive subdomain enumeration |
| Assetfinder | Certificate log and DNS discovery |
| Amass | Comprehensive attack surface mapping |
| CRT.sh | Certificate transparency logs |
| DNSDumpster | DNS record aggregation |
| HTTPX | HTTP probing and response validation |
| Aquatone | Visual reconnaissance and screenshots |
| WhatWeb / Wappalyzer | Technology fingerprinting |
| Google Dorking | Exposed file and indexed asset discovery |
| GitHub Dorking | Credential leak analysis |

---

## Key Findings
| # | Finding | Severity |
|---|---------|----------|
| 01 | 70 unique subdomains discovered | Informational |
| 02 | 23 live HTTP 200 endpoints confirmed | Medium |
| 03 | Server technology stack disclosed via DNS | Medium |
| 04 | Legacy and dormant subdomains identified | Low |
| 05 | Exposed PDF documents indexed publicly | Medium |
| 06 | robots.txt exposing internal path structure | Low-Medium |
| 07 | Mail records visible in DNS | Informational |
| 08 | No credential leaks found in GitHub dorking | Negative finding |

---

## Key Lesson
Sir's method: check each URL manually one by one.  
My approach: pipe all subdomains through Aquatone 
for automated visual recon — mapped entire attack 
surface in 55 seconds.

httpx sorted results by status code (200/301/403/404) 
giving instant live host validation across 70 subdomains.

---

## Full Report
Confidential — submitted internally to Cyberster 
Red Teaming Internship Program.
