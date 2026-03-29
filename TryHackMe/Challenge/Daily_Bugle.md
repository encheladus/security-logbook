# Daily Bugle — TryHackMe

**Date:** 2026-03-29  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

A Joomla-based machine focused on exploiting a known SQL injection vulnerability to gain credentials, followed by lateral movement and privilege escalation using misconfigured sudo permissions.

---

## 2) Initial hypothesis

- Identify web application version and look for known vulnerabilities  
- Exploit potential SQL injection in Joomla  
- Extract and crack password hashes  
- Reuse credentials across services (web → SSH)  
- Search for privilege escalation vectors (sudo misconfigurations)  

---

## 3) Tools used

- Nmap  
- Dirsearch  
- WhatWeb  
- John the Ripper  
- Hydra (optional testing)  
- Netcat  
- GTFOBins  

---

## 4) Approach (high level)

- Performed service enumeration to identify exposed services  
- Identified Joomla CMS and focused on version fingerprinting  
- Discovered vulnerable Joomla version through exposed documentation files  
- Leveraged a known SQL injection vulnerability to extract database data  
- Cracked retrieved password hash to obtain valid credentials  
- Enumerated system files and extracted database credentials from configuration  
- Reused credentials to gain SSH access as a legitimate user  
- Checked sudo permissions and identified a misconfigured binary  
- Exploited `yum` via plugin loading to execute code as root  

---

## 5) Results / Evidence (sanitized)

- Joomla version identified as vulnerable to SQL injection  
- Database extraction revealed user credentials and hashed password  
- Hash successfully cracked, providing valid login credentials  
- Configuration file exposed additional credentials reused for SSH access  
- Achieved stable shell access as a system user  
- Sudo misconfiguration allowed execution of package manager as root  
- Privilege escalation achieved through plugin abuse  
- Final state: full system compromise with root-level access  

---

## 6) Recommended remediation

- Keep CMS and plugins updated to patched versions  
- Implement proper input validation to prevent SQL injection  
- Restrict access to sensitive files such as documentation and configuration files  
- Avoid storing plaintext or weakly protected credentials in configuration files  
- Enforce strong password policies and prevent reuse  
- Apply least privilege principles for sudo permissions  
- Restrict execution of package managers or similar binaries with elevated privileges  

---

## 7) Lessons learned

- Version fingerprinting is key to efficient exploitation  
- SQL injection can be leveraged for data extraction, not just authentication bypass  
- Credential reuse is a common and critical weakness  
- Configuration files are high-value targets during post-exploitation  
- Misconfigured sudo permissions can lead to full system compromise  
- Chaining vulnerabilities is essential in real-world scenarios  

---

## 8) Links / Resources

- Room / machine link: https://tryhackme.com/room/dailybugle  
- Useful docs: https://book.hacktricks.xyz/  
- Notion / detailed write-up (public): N/A  

---
