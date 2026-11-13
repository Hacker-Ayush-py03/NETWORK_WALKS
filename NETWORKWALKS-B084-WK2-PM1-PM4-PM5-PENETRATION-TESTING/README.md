
# 🔴 WEEK 02 — PENETRATION TESTING & FOOTPRINTING

<p align="center">
  <strong>Cybersecurity | Ethical Hacking | Information Gathering | Network Security</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Project-Week%2002-red?style=for-the-badge" alt="Week 2"/>
  <img src="https://img.shields.io/badge/Platform-Kali%20Linux-black?style=for-the-badge&logo=kalilinux" alt="Kali Linux"/>
  <img src="https://img.shields.io/badge/Focus-Penetration%20Testing-darkred?style=for-the-badge" alt="Penetration Testing"/>
  <img src="https://img.shields.io/badge/Status-Documentation%20Prepared-success?style=for-the-badge" alt="Status"/>
</p>

---

## 📌 Project Overview

This repository contains my **Week 02 Cybersecurity Project Work**, focused on website footprinting, OSINT-based reconnaissance, DNS enumeration, web security assessment, and network scanning.

The practical activities were performed using Kali Linux and various industry-standard cybersecurity tools.

The assessment focuses on collecting publicly observable information, identifying exposed services, analysing security configurations, and documenting potential security concerns through structured technical reports.

**Target Domain:** `networkwalks.com`

**Target IP Observed:** `192.232.216.135`

**Operating System:** Kali Linux

---

## 📚 Completed Project Modules

As part of the Week 02 assignment, the following three modules were selected and documented.

| Module | Project | Status |
|---|---|---|
| W2-PM1 | Footprinting with Multiple Kali Tools | Completed |
| W2-PM4 | theHarvester Based Footprinting Attacks | Completed |
| W2-PM5 | Zenmap Based Network Scanning | Documented |

---

# 🔎 W2-PM1 — Footprinting with Multiple Kali Tools

### Objective

To perform detailed website footprinting and information gathering using multiple Kali Linux tools to identify domain registration details, website technologies, DNS records, HTTP configurations, WAF technologies, and publicly observable infrastructure information.

### Tools Used

| No. | Tool | Assessment Area |
|---|---|---|
| 01 | WHOIS | Domain Registration & Ownership Information |
| 02 | WhatWeb | Web Technology Fingerprinting |
| 03 | Nslookup | DNS Enumeration |
| 04 | cURL | HTTP Headers, Redirects & TLS Analysis |
| 05 | Wafw00f | Web Application Firewall Detection |
| 06 | OWASP Amass | Passive Attack Surface Reconnaissance |

### Key Findings

- Identified domain registrar, registration dates, domain status, and registration privacy information.
- Detected Apache, WordPress, Bootstrap, jQuery, and other publicly identifiable website technologies.
- Enumerated A, AAAA, NS, MX, TXT, SOA, CNAME, and reverse DNS records.
- Identified email infrastructure and publicly observable DNS configuration.
- Analysed HTTP response headers, redirects, cookies, request timing, and TLS certificate metadata.
- Identified ModSecurity (SpiderLabs) through Wafw00f fingerprinting.
- Compared normal HTTP response behaviour with modified requests, observing HTTP 200 and HTTP 406 responses.
- Executed OWASP Amass v5.1.3 passive reconnaissance; no additional subdomains were displayed in the observed runs.

### Evidence Directory

```text
images/
├── WHOIS
├── WhatWeb
├── NSLOOKUP/
├── cURL/
├── Wafw00f
└── Amass
```

---

# 🌐 W2-PM4 — theHarvester Based Footprinting

### Objective

To perform OSINT-based reconnaissance and identify publicly indexed subdomains, hostnames, IP address associations, and infrastructure relationships using multiple publicly available data sources.

### Tool Information

- **Tool:** theHarvester
- **Version:** 4.11.1
- **Platform:** Kali Linux
- **Target:** networkwalks.com

### Data Sources Used

- CRT.sh
- CertSpotter
- DuckDuckGo
- Yahoo
- Baidu
- Common Crawl
- HackerTarget
- RapidDNS
- AlienVault OTX
- URLScan
- Wayback Archive
- SubdomainCenter

### Key Findings

1. Certificate Transparency sources returned the wildcard hostname pattern `*.networkwalks.com`.
2. Search-engine-based enumeration did not return additional hosts, emails, or IP addresses in the observed run.
3. Multi-source reconnaissance returned hostname and infrastructure associations.
4. Historical/public-indexed reconnaissance returned 14 hostname candidates.
5. DNS resolution returned 20 hostname-to-address or hostname-to-hostname mapping entries.
6. Eight unique subdomains were identified in the combined resolution results.
7. The IPv4 address `192.232.216.135` was associated with multiple discovered hostnames.
8. DNS verification using DIG confirmed A-record and CNAME relationships for several hostnames.
9. WHOIS analysis identified HostGator.com LLC as the registered organization for the containing IP network allocation.
10. Reverse DNS returned `192-232-216-135.unifiedlayer.com`.

### Discovered Subdomains

| No. | Hostname |
|---|---|
| 01 | autodiscover.networkwalks.com |
| 02 | cpanel.networkwalks.com |
| 03 | cpcalendars.networkwalks.com |
| 04 | cpcontacts.networkwalks.com |
| 05 | mail.networkwalks.com |
| 06 | webdisk.networkwalks.com |
| 07 | webmail.networkwalks.com |
| 08 | www.networkwalks.com |

*Note: These are reconnaissance candidates. DNS verification did not return matching records for cpcalendars and cpcontacts in the observed queries.*

### Evidence Directory

```text
images/
└── theHarvester/
```

---

# 🖥️ W2-PM5 — Zenmap Based Network Scanning

### Objective

To examine externally observable network services, identify open and filtered TCP ports, and document potential security exposure using Nmap and Zenmap.

### Tool Information

- **Tool:** Nmap / Zenmap
- **Nmap Version:** 7.99
- **Platform:** Kali Linux
- **Observed Target IP:** 192.232.216.135

### Initial Scan Findings

The initial SYN Stealth Scan identified 15 open TCP ports among the 1,000 scanned ports.

| Port | Common Service Association |
|---|---|
| 21 | FTP |
| 22 | SSH |
| 25 | SMTP |
| 26 | Unknown / Custom |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 465 | SMTPS |
| 587 | SMTP Submission |
| 993 | IMAPS |
| 995 | POP3S |
| 2222 | Unknown / Alternate Service |
| 3306 | MySQL |

### Additional Scan Observations

- Service detection was initiated for 15 services, but the detailed service-version table was not available in the collected evidence.
- Initial operating-system detection did not produce a conclusive fingerprint.
- NSE scanning was initiated; some scripts reported missing parameters or API credentials.
- A subsequent service-detection attempt reported the host as down.
- A later `-Pn -sV` execution treated the host as up but reported all 1,000 scanned TCP ports as filtered.

### Security Assessment

The exposed ports indicate potential web, email, DNS, remote administration, and database-related service exposure.

These observations require appropriate configuration review. Open or filtered ports alone do not establish exploitable vulnerabilities.

No confirmed CVE or successful exploitation was established from the available scan evidence.

### Evidence Directory

```text
images/
└── Zenmap
```

---

## 📊 Consolidated Assessment Summary

| Assessment Area | Key Observation |
|---|---|
| Domain Footprinting | Registration information collected |
| Technology Fingerprinting | Apache and WordPress identified |
| DNS Enumeration | Multiple DNS record types examined |
| HTTP Analysis | Response headers and TLS metadata collected |
| WAF Detection | ModSecurity (SpiderLabs) identified |
| Passive Reconnaissance | Wildcard and subdomain candidates identified |
| Infrastructure Mapping | IPv4 and hostname relationships examined |
| Network Scanning | 15 open TCP ports observed in initial scan |
| Service Identification | Detailed version evidence incomplete |
| Vulnerability Validation | No confirmed exploitable vulnerability established |

---

## 🛠️ Tools & Technologies

<p align="center">
  <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white"/>
  <img src="https://img.shields.io/badge/WHOIS-Information_Gathering-red?style=flat-square"/>
  <img src="https://img.shields.io/badge/WhatWeb-Fingerprinting-darkred?style=flat-square"/>
  <img src="https://img.shields.io/badge/NSLookup-DNS_Analysis-black?style=flat-square"/>
  <img src="https://img.shields.io/badge/cURL-HTTP_Analysis-red?style=flat-square"/>
  <img src="https://img.shields.io/badge/Wafw00f-WAF_Detection-darkred?style=flat-square"/>
  <img src="https://img.shields.io/badge/OWASP_Amass-Reconnaissance-black?style=flat-square"/>
  <img src="https://img.shields.io/badge/theHarvester-OSINT-red?style=flat-square"/>
  <img src="https://img.shields.io/badge/Nmap-Network_Scanning-darkred?style=flat-square"/>
  <img src="https://img.shields.io/badge/Zenmap-Network_Analysis-black?style=flat-square"/>
</p>

---


## 📄 Detailed Project Report

The complete Week 02 penetration-testing report includes:

- Objectives and methodologies
- Executed commands
- Technical findings and observations
- Security analysis
- Potential risk areas
- Recommendations and conclusions
- Screenshot evidence references

**Report:** `W2_PM_FINAL_Penetration_Testing_Report.pdf`

---

## 🔐 Ethical Considerations & Disclaimer

This repository is maintained for educational and cybersecurity learning purposes.

The documented activities focus on footprinting, passive reconnaissance, DNS information gathering, HTTP analysis, and network scanning observations.

The identification of an IP address through DNS does not establish ownership of that IP or permission to test the entire hosting infrastructure.

The observed IP address belongs to a network allocation registered to a hosting provider. Any further active testing, vulnerability scanning, or exploitation requires explicit authorization and an agreed testing scope.

No confirmed exploitable vulnerability is claimed without supporting validation evidence.

Sensitive credentials, API keys, private information, and unredacted session cookies should not be published in this repository.

---

## 👨‍💻 Project Information

**Project:** Week 02 – Penetration Testing & Footprinting

**Repository:** NETWORK_WALKS

**Modules Completed:** W2-PM1, W2-PM4, W2-PM5

**Environment:** Kali Linux

---

<p align="center">
  <strong>Learning • Reconnaissance • Analysis • Documentation</strong>
  <br/>
  <sub>Week 02 Cybersecurity Project — NETWORK_WALKS</sub>
</p>
