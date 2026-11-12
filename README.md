# Week 2 Project Report: Footprinting and Network Scanning

**Programme:** Cybersecurity Internship  
**Week:** 2  
**Date:** 12 November 2026

**Modules Completed:**
- W2-PM4: theHarvester-based Footprinting Attacks
- W2-PM5: Zenmap-based Network Scanning

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [W2-PM4: theHarvester-Based Footprinting](#2-w2-pm4-theharvester-based-footprinting)
3. [W2-PM5: Zenmap-Based Network Scanning](#3-w2-pm5-zenmap-based-network-scanning)
4. [Security Observations](#4-security-observations)
5. [Recommendations](#5-recommendations)
6. [Conclusion](#6-conclusion)
7. [Evidence and Appendices](#7-evidence-and-appendices)

---

## 1. Introduction

This report documents the practical activities completed during Week 2 of the cybersecurity internship.

The exercises focused on footprinting and network scanning. The objective was to understand how publicly available information can be collected about a domain and how network scanning can identify active hosts, open ports, and running services.

Two project modules were completed:

- **W2-PM4:** theHarvester-based Footprinting Attacks.
- **W2-PM5:** Zenmap-based Network Scanning.

The activities documented in this report reflect the commands executed and the results observed during the exercises.

---

## 2. W2-PM4: theHarvester-Based Footprinting

### 2.1 Objective

The objective was to use theHarvester to demonstrate how publicly available information about a domain can be collected from different search sources.

### 2.2 Tools and Environment

| Item | Details |
|---|---|
| Operating System | Kali Linux |
| Tool | theHarvester 4.8.2 |
| Demonstration Domain | example.com |
| Output Formats | JSON and XML |

### 2.3 Activities Performed

#### Step 1: Verify theHarvester Installation

The following command was executed:

```bash
theHarvester -h
```

The tool displayed its help information and confirmed that version 4.8.2 was installed.

**Evidence:** `screenshots/W2-PM4_Step01_Tool_Verification.png`

#### Step 2: Reconnaissance Using CRT.sh

Command:

```bash
theHarvester -d example.com -b crtsh -l 50
```

The request returned an HTTP 502 error. The source responded with HTML instead of the JSON format expected by the tool.

No IP addresses, email addresses, people, or hosts were returned.

This was recorded as a source/request error, not as evidence that the domain has no publicly available information.

**Evidence:** `screenshots/W2-PM4_Step02_CRTsh_Reconnaissance.png`

#### Step 3: Reconnaissance Using DuckDuckGo

Command:

```bash
theHarvester -d example.com -b duckduckgo -l 50
```

Results:

| Result Type | Count |
|---|---:|
| IP Addresses | 0 |
| Email Addresses | 0 |
| People | 0 |
| Hosts | 0 |

The search completed successfully but returned no records.

**Evidence:** `screenshots/W2-PM4_Step03_DuckDuckGo_Reconnaissance.png`

#### Step 4: Reconnaissance Using Yahoo

Command:

```bash
theHarvester -d example.com -b yahoo -l 50
```

Results:

| Result Type | Count |
|---|---:|
| IP Addresses | 0 |
| Email Addresses | 0 |
| People | 0 |
| Hosts | 0 |

The search completed successfully but returned no records.

**Evidence:** `screenshots/W2-PM4_Step04_Yahoo_Reconnaissance.png`

#### Step 5: Export Reconnaissance Results

The following command was executed to export the DuckDuckGo results:

```bash
theHarvester -d example.com -b duckduckgo -l 50 -f W2-PM4_DuckDuckGo
```

The tool successfully generated two files:

- `W2-PM4_DuckDuckGo.json`
- `W2-PM4_DuckDuckGo.xml`

The files were verified using:

```bash
ls -lh W2-PM4_DuckDuckGo.*
```

The output confirmed that both files existed.

| File | Size |
|---|---:|
| JSON | 88 bytes |
| XML | 161 bytes |

**Evidence:** `screenshots/W2-PM4_Step05_Results_Export.png`

### 2.4 Results and Findings

The exercise demonstrated how theHarvester can query different information sources and export collected results into structured formats.

The following observations were made:

- CRT.sh returned an HTTP 502 error.
- DuckDuckGo returned zero IP addresses, email addresses, people, and hosts.
- Yahoo returned zero IP addresses, email addresses, people, and hosts.
- The DuckDuckGo results were successfully exported into JSON and XML formats.

The results are limited to the sources queried during this exercise. They do not prove that no publicly available information exists about the domain.

### 2.5 Limitations

- `example.com` was used as a demonstration domain because no target domain was provided in the project instructions.
- The selected search sources returned no footprinting records.
- CRT.sh returned an HTTP 502 error.
- No API-based sources were configured.

---

## 3. W2-PM5: Zenmap-Based Network Scanning

### 3.1 Objective

The objective was to use Zenmap to scan an authorized lab host, identify open ports, and determine which services were running.

### 3.2 Tools and Environment

| Item | Details |
|---|---|
| Scanning Machine | Kali Linux |
| Target Machine | Ubuntu Server |
| Target IP Address | 192.168.110.3 |
| Scanning Interface | Zenmap |
| Scan Engine | Nmap |
| Scan Profile | Intense Scan |
| Virtual Network | VirtualBox Host-only Network |

### 3.3 Activities Performed

#### Step 1: Launch Zenmap

Zenmap was opened on Kali Linux.

**Evidence:** `screenshots/W2-PM5_Step01_Zenmap_Interface.png`

#### Step 2: Configure and Execute the Scan

The target IP address was entered into Zenmap:

```text
192.168.110.3
```

The **Intense Scan** profile was selected and the scan was executed.

Zenmap completed the scan and reported that the target host was active.

**Evidence:** `screenshots/W2-PM5_Step04_Zenmap_Intense_Scan.png`

### 3.4 Network Scanning Results

The scan identified the following:

| Item | Result |
|---|---|
| Target IP | 192.168.110.3 |
| Host Status | Up |
| Open TCP Ports | 1 |
| Open Port | 80/tcp |
| Service | HTTP |
| Service Version | Apache httpd 2.4.58 (Ubuntu) |
| Closed TCP Ports | 999 |
| Network Distance | 1 hop |

The scan identified the following service:

```text
80/tcp open http Apache httpd 2.4.58 ((Ubuntu))
```

The HTTP page title was:

```text
Apache2 Ubuntu Default Page: It works
```

The scan also identified these supported HTTP methods:

- GET
- POST
- OPTIONS
- HEAD

Nmap suggested a Linux operating system family during OS detection. This was an estimate rather than a confirmed operating system identification.

### 3.5 HTTP Service Verification

To independently verify the discovered HTTP service, the following command was executed from Kali:

```bash
curl -I http://192.168.110.3
```

The server returned:

```text
HTTP/1.1 200 OK
Date: Thu, 12 Nov 2026 14:08:48 GMT
Server: Apache/2.4.58 (Ubuntu)
Last-Modified: Thu, 05 Mar 2026 03:08:16 GMT
Content-Length: 10671
Content-Type: text/html
```

The response confirmed that the Apache web server was responding successfully to HTTP requests.

**Evidence:** `screenshots/W2-PM5_Step05_HTTP_Service_Verification.png`

### 3.6 Saved Scan Results

The Zenmap scan was saved in XML format:

```text
W2-PM5_Intense_Scan.xml
```

This file preserves the scan results for further review.

---

## 4. Security Observations

The exercises produced the following observations:

1. theHarvester can query different public information sources, but results depend on source availability and the information returned.
2. Zenmap identified the Ubuntu lab server as an active host.
3. TCP port 80 was open and exposed an Apache HTTP service.
4. The Apache service returned an HTTP 200 OK response during independent verification.
5. Nmap's operating system identification was an estimate and should not be treated as conclusive.

An open port or successful HTTP response does not automatically establish that a vulnerability exists. Further assessment would be required to determine whether any security weaknesses are present.

---

## 5. Recommendations

Based on the observations from the network scan:

- Keep Apache and the Ubuntu operating system updated.
- Review exposed services and ensure only necessary ports are accessible.
- Restrict access to services that are not required.
- Review Apache configuration and security settings.
- Monitor web server logs for unusual requests.
- Validate scan findings before classifying them as vulnerabilities.

These are general recommendations based on the service identified during the scan. No exploitation or vulnerability confirmation was performed.

---

## 6. Conclusion

The Week 2 exercises provided practical experience in footprinting and network scanning.

Using theHarvester, I queried multiple information sources and exported the results into JSON and XML formats. The selected sources returned no footprinting records for the demonstration domain, while CRT.sh returned an HTTP 502 error.

Using Zenmap, I scanned an Ubuntu lab server, identified an active host, discovered an open TCP port 80, and identified Apache HTTP Server 2.4.58.

I then used cURL to independently verify that the HTTP service returned a successful response.

These exercises improved my understanding of information gathering, host discovery, port scanning, service identification, and the importance of validating results before drawing security conclusions.

---

## 7. Evidence and Appendices

The screenshots should be stored in a folder named `screenshots` alongside this Markdown report.

### W2-PM4: theHarvester

| Evidence | File |
|---|---|
| Tool Verification | `screenshots/W2-PM4_Step01_Tool_Verification.png` |
| CRT.sh Reconnaissance | `screenshots/W2-PM4_Step02_CRTsh_Reconnaissance.png` |
| DuckDuckGo Reconnaissance | `screenshots/W2-PM4_Step03_DuckDuckGo_Reconnaissance.png` |
| Yahoo Reconnaissance | `screenshots/W2-PM4_Step04_Yahoo_Reconnaissance.png` |
| Results Export | `screenshots/W2-PM4_Step05_Results_Export.png` |

### W2-PM5: Zenmap

| Evidence | File |
|---|---|
| Zenmap Interface | `screenshots/W2-PM5_Step01_Zenmap_Interface.png` |
| Intense Scan Results | `screenshots/W2-PM5_Step04_Zenmap_Intense_Scan.png` |
| HTTP Service Verification | `screenshots/W2-PM5_Step05_HTTP_Service_Verification.png` |
| Saved Scan | `W2-PM5_Intense_Scan.xml` |

### Exported theHarvester Results

- `W2-PM4_DuckDuckGo.json`
- `W2-PM4_DuckDuckGo.xml`

---

**End of Report**
