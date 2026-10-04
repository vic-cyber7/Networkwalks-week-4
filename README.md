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

```
### Findings & Evidences
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/4f154c9e-dfe0-403f-aee0-285f0661b39b" />

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/8084564b-927b-4e44-aea9-951e548acb16" />

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/bdfe5b5b-39e9-4618-9b37-034e260d551a" />

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/9f9c2b5b-a0c5-44dc-a9b6-776c6217af8a" />

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/48d43f77-96ab-42c9-ac0f-17d83c831199" />

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/64e8cf98-bf6f-4fe5-9ad6-224fc520f3e4" />

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/41520e00-9963-4c45-a3c1-41877e47a9da" />

### Finding 2 — Unprotected Directory Exposing a Full Database Backup

**Risk Rating: 🔴 Critical**

The `/old/` directory identified through `robots.txt` was accessible without authentication and had **directory listing enabled**. The directory exposed a complete, downloadable SQL database backup named `mediroza_db_backup_2019.sql`, potentially allowing unauthorized access to sensitive application data.

<img width="1920" height="1008" alt="Screenshot 2026-10-03 231641" src="https://github.com/user-attachments/assets/e6fdd88a-6985-4dcf-93df-bb1d8e955b73" />

<img width="1920" height="1008" alt="Screenshot 2026-10-03 232041" src="https://github.com/user-attachments/assets/a66a2df8-6d0b-42cd-b4ae-7ee4a758067d" />

The exposed backup contained two complete internal database tables, effectively satisfying **Milestone 3** of the assessment:

- **`staff`** — 30 employee records containing names, job titles, departments, email addresses, phone numbers, national ID numbers, monthly salaries, and employment start dates.
- **`shareholders`** — 10 records containing shareholder names, ownership percentages, shares held, and share classes.

### Sample — Redacted Staff Salary Data

| **Role** | **Monthly Salary (ZAR)** |
|---|---:|
| Medical Director | R160,000 |
| Chief Financial Officer | R152,000 |
| Chief Pathologist | R138,000 |
| ... | ... |
| Ward Clerk | R21,000 |

### Sample — Redacted Shareholder Data

| **Shareholder** | **Ownership %** |
|---|---:|
| Dr. Rajesh Naidoo | 18.0% |
| Cedar Health Holdings (Pty) Ltd | 15.0% |
| Dr. Johan van der Merwe | 12.0% |
| ... | ... |

> **Data Handling Notice:** Full unredacted records, including national identification numbers, have been excluded from this public report. Sensitive information was submitted separately and privately to the instructor in accordance with responsible-disclosure practices.

### Impact

The exposed backup resulted in **unauthorized disclosure of sensitive employee and corporate information without authentication**. This represents a significant risk to employee privacy, organizational confidentiality, and the overall security posture of the application.

### Finding 3 — Staff Portal SQL Injection Assessment (No Vulnerability Confirmed)
**Risk Rating: 🟡 Low — No Vulnerability Confirmed**

The `/staff/login.php` authentication form was tested for SQL injection using a standard authentication-bypass payload. The attempt was unsuccessful, and no authentication bypass was achieved through this specific input.

<img width="1920" height="1008" alt="Screenshot 2026-10-03 232950" src="https://github.com/user-attachments/assets/3aec97cb-3a41-41ea-ae1c-93f7b25e82ca" />

The staff login endpoint appears to properly handle or parameterize the tested input. A separate successful SQL injection vector is documented in **Finding 4**.

### Finding 4 — SQL Injection Resulting in Patient Portal Authentication Bypass

**Risk Rating: 🔴 Critical**

Unlike the staff portal, the `/patient/login.php` endpoint was found to be vulnerable to SQL injection. An initial test payload (`admin' --`) triggered a raw MySQL syntax error, indicating that user-supplied input was being inserted directly into the backend SQL query without adequate sanitization or parameterization.

<img width="1920" height="1008" alt="Screenshot 2026-10-03 233904" src="https://github.com/user-attachments/assets/3361201b-dbd8-4528-9a21-57e0464f90b2" />

A refined SQL injection test successfully bypassed the authentication control, allowing access to the patient portal without valid credentials. The portal exposed three downloadable, patient-specific pathology laboratory reports. The test credentials used during the assessment were synthetic and randomly generated for the training environment.

<img width="1920" height="1008" alt="Screenshot 2026-10-03 233917" src="https://github.com/user-attachments/assets/2a292d3a-3b65-409d-abf3-339b4151a0a3" />

**Impact:** The vulnerability allowed complete authentication bypass on a portal designed to protect confidential medical information. An unauthorized user could potentially access patient laboratory reports without valid credentials. This finding satisfies **Milestone 1** of the engagement.


Finding 5 — Weak Password Protection on "Encrypted" Patient PDFs
Risk rating: 🟠 High

All three retrieved PDF lab reports were password-protected (PDF R3/128-bit encryption), but the passwords chosen were trivially weak:

Report	Patient (synthetic)	Password	Cracking Method	Time to Crack
patient_report_1.pdf	Sipho Dlamini	123456	Networkwalks Password Cracker (100-word built-in list)	< 1 second
patient_report_2.pdf	Priya Reddy	password	Networkwalks Password Cracker (100-word built-in list)	< 1 second
patient_report_3.pdf	Emily Thompson	!@#$%^&	hashcat + rockyou.txt (14.3M password wordlist)	Seconds
Process:

 Extracted each PDF's crackable hash via Networkwalks Hash Calculator (pdf2john-compatible)
 Reports 1 & 2 — cracked instantly with built-in 100-word dictionary:

## Evidences
<img width="1920" height="1008" alt="Screenshot 2026-10-03 234935" src="https://github.com/user-attachments/assets/849ba7bd-8d9b-4e6e-96b8-5ba787e19114" />

<img width="1920" height="1080" alt="Screenshot 2026-10-03 235149" src="https://github.com/user-attachments/assets/fadc4998-24dd-488c-94cc-f411c0a784b2" />

<img width="1920" height="1008" alt="Screenshot 2026-10-03 235433" src="https://github.com/user-attachments/assets/264c0d5d-df91-49f0-9ad6-78a1681a7ca5" />

<img width="1920" height="1008" alt="Screenshot 2026-10-03 235443" src="https://github.com/user-attachments/assets/c1d28d0c-ac43-47fe-aecf-e980c862b2d6" />


<img width="1920" height="1008" alt="Screenshot 2026-10-03 235643" src="https://github.com/user-attachments/assets/694cd2fd-fba1-46f8-ab96-7bed85dd52db" />

<img width="1920" height="1008" alt="Screenshot 2026-10-03 235811" src="https://github.com/user-attachments/assets/9edd150c-8972-49e8-9791-b11845c45d0f" />

<img width="1920" height="1008" alt="Screenshot 2026-10-04 000019" src="https://github.com/user-attachments/assets/74ee6a46-51db-42d4-b40c-d60da9e9509b" />

<img width="1920" height="1008" alt="Screenshot 2026-10-04 000041" src="https://github.com/user-attachments/assets/0d0b2815-a669-43c8-ad1e-7a33c643dfe3" />


### Report 3 — Additional Password Cracking Analysis

Report 3 required a different approach from the first two files. The password was **not recovered using the Networkwalks Password Cracker's built-in dictionary**, and a standard John the Ripper default run was also unsuccessful.

This demonstrated that different password-protected files may require different wordlists and cracking approaches. The assessment was therefore escalated to `hashcat` using the `rockyou.txt` wordlist, containing approximately **14.3 million candidate passwords**. The password was successfully recovered within seconds.

```bash
hashcat -m 10500 hashcat_ready.txt /usr/share/wordlists/rockyou.txt
```

<img width="1920" height="1008" alt="image" src="https://github.com/user-attachments/assets/e5f1ca1c-7c2e-4ff0-a059-47b048c446f8" />

<img width="816" height="465" alt="image" src="https://github.com/user-attachments/assets/17f3f5ff-e645-409c-a60d-cd0a1fa3b5f0" />


<img width="1280" height="654" alt="image" src="https://github.com/user-attachments/assets/e85b1def-9dff-4833-bb75-79b58f02ab58" />


**Impact:** The weak password protection provided only limited security for the patient documents. Even without the SQL injection identified in Finding 4, an attacker with access to the files could potentially recover the passwords using commonly available password-cracking tools.

---

## 4️⃣ Risk Summary

| **#** | **Finding** | **Risk Rating** |
|---:|---|---|
| **1** | Reconnaissance Surface / `robots.txt` Disclosure | Informational |
| **2** | Unprotected `/old/` Directory Exposing Database Backup | 🔴 Critical |
| **3** | Staff Portal SQL Injection Testing — Unsuccessful | 🟡 Low |
| **4** | Patient Portal SQL Injection Authentication Bypass | 🔴 Critical |
| **5** | Weak Password Protection on Encrypted Patient PDFs | 🟠 High |

---

## 5️⃣ Recommendations & Remediation

| **Finding** | **Recommended Remediation** |
|---|---|
| **`robots.txt` Disclosure** | Do not use `robots.txt` as a security control. Sensitive directories should be protected through proper authentication and access controls. |
| **Exposed `/old/` Backup** | Disable directory listing and remove database backups from web-accessible directories. Store backups outside the web root and review the server for other exposed or forgotten files. |
| **Patient Portal SQL Injection** | Use parameterized queries or prepared statements for all database operations. Apply consistent input validation and secure coding practices across every application endpoint. |
| **Weak PDF Passwords** | Enforce strong, randomly generated passwords for protected documents. Where possible, use authenticated access-controlled downloads instead of relying solely on PDF-level password protection. |
| **General Security** | Perform regular automated and manual security assessments. Routine vulnerability scanning and configuration reviews could help identify exposed files and other weaknesses before they are chained together. 


## 🎓 Internship Completion Overview

This capstone project represents the completion of a **4-week cybersecurity internship with Network Walks Academy**, covering practical cybersecurity skills from lab setup and reconnaissance to password security and penetration testing.

- **Week 1:** VirtualBox Lab Setup — Kali Linux, Windows 10, and Networking Fundamentals
- **Week 2:** Footprinting & Reconnaissance — GHDB, Maltego, theHarvester, Nmap, and other reconnaissance tools
- **Week 3:** Password Security & Cracking — John the Ripper and Networkwalks password-cracking tools
- **Week 4:** Black-Box Penetration Testing — Full assessment, vulnerability analysis, exploitation, and reporting












