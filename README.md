# Penetration Testing Project

# 🔐 Mediroza General Hospital — Penetration Testing Capstone

### Network Walks Academy | Batch B083 | 4-Week Cybersecurity Internship

A full **black-box penetration testing assessment** conducted against a purpose-built training environment as the final capstone project of my 4-week cybersecurity internship.

The assessment focused on identifying vulnerabilities, analyzing their potential impact, and documenting the findings with practical security recommendations.

> ⚠️ **Training Environment Notice**
>
> This project was conducted against a fictional environment created by Network Walks Academy for educational purposes. All names, records, patient information, and other data used in the assessment are **synthetic and created solely for cybersecurity training**.

---
## 👤 Analyst Credentials

| **Field** | **Details** |
|---|---|
| **Practitioner** | Falusi Victor |
| **Role / Track** | Cybersecurity Specialist / Networkwalks Trainee |
| **Cohort** | Batch B083 |
| **Date Completed** | 1 October 2026 |

## 📌 Project Overview

**Target:** `medirozahospital.com`  
**Assessment Type:** Black-Box Penetration Test  
**Authorization:** Written authorization provided  
**Program:** Network Walks Academy  
**Batch:** B083  
**Duration:** 4 Weeks  
**Project Type:** Internship Capstone

---

## 🎯 Objectives

- Identify security vulnerabilities within the target environment
- Assess the potential impact of discovered vulnerabilities
- Demonstrate realistic attack paths in a controlled environment
- Document findings and supporting evidence
- Provide recommendations for improving security posture

---

## 🛠️ Areas Covered

- Reconnaissance & Information Gathering
- Web Application Security
- Directory & File Enumeration
- SQL Injection Testing
- Authentication Bypass
- Sensitive Data Exposure
- Password Security
- Vulnerability Analysis
- Risk Assessment & Reporting

# 1. Executive Summary 
## Executive Summary

The assessment uncovered a **critical chain of vulnerabilities** within the Mediroza General Hospital web infrastructure. The attack path began with a misconfigured directory and progressed to unauthorized access to sensitive information, including **patient laboratory reports, internal staff salary records, and shareholder ownership data**.

**Overall Risk Rating: Critical**

The primary issues identified were:

- A publicly accessible backup file exposed through an unprotected `/old/` directory.
- A **SQL injection vulnerability** in the patient portal login form that enabled authentication bypass without valid credentials.
- Three patient PDF reports were protected with weak passwords. Two of the passwords were successfully cracked in under a second using a 100-word dictionary.

These findings demonstrate how multiple security weaknesses can be chained together to significantly increase the potential impact of a compromise.

# 2. Scope and Methodology 
## 📋 Engagement Details

| **Field** | **Details** |
|---|---|
| **Client** | Mediroza General Hospital |
| **Target** | `https://medirozahospital.com` |
| **Assessment Type** | Black-Box Penetration Test |
| **Duration** | 3 Days |
| **Authorization** | Written authorization provided by the client as outlined in the engagement brief |
| **Scope** | Testing was restricted to the specified target domain |
| **Restrictions** | No social engineering, denial-of-service testing, or activity outside the agreed scope |

### 🛠️ Tools & Techniques

- `whois`
- `whatweb`
- `nslookup`
- `curl`
- `gobuster`
- Manual browser-based testing
- Manual SQL injection testing
- Networkwalks Hash Calculator
- Networkwalks Password Cracker
- `hashcat`
- `rockyou.txt`
- `exiftool`

  ## 🔍 Methodology
The assessment followed a structured **black-box web application testing methodology**, progressing through the following phases:

**Passive Reconnaissance → Directory & Content Discovery → Authentication Testing → Vulnerability Exploitation → Data Analysis → Reporting**

# 3. Finding and Proof of Exploitation
## Finding 1 — Reconnaissance & Attack Surface Mapping

Initial passive and active reconnaissance identified the target's **LiteSpeed web server**, IP address (`199.188.201.16`), and a `robots.txt` file containing references to three potentially sensitive directories:

- `/patient/`
- `/staff/`
- `/old/`

Although `robots.txt` is intended to control search-engine crawling, it also revealed these paths and provided useful information for further assessment of the application's attack surface.

### 🔎 Reconnaissance & Enumeration Commands

```bash
# WHOIS lookup
whois medirozahospital.com

# Web technology fingerprinting
whatweb medirozahospital.com

# DNS lookup
nslookup medirozahospital.com

# Inspect HTTP response headers
curl -I https://medirozahospital.com

# Retrieve robots.txt
curl https://medirozahospital.com/robots.txt

# Directory and content discovery
gobuster dir -u https://medirozahospital.com \
-w /usr/share/seclists/Discovery/Web-Content/common.txt
