# Skynet — TryHackMe

**Date:** 2026-03-25  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

A vulnerable Terminator-themed Linux machine focused on chaining multiple enumeration findings into full system compromise. The objective was to practice pivoting across services and privilege escalation techniques.

---

## 2) Initial hypothesis

- Enumerate exposed services to identify entry points  
- Target SMB for potential anonymous access and sensitive data leakage  
- Attempt credential reuse across services (mail, SMB, web)  
- Explore web application for vulnerabilities (CMS exploitation)  
- Look for privilege escalation via misconfigurations (cron jobs, scripts)  

---

## 3) Tools used

- Nmap  
- Gobuster  
- SMBMap / smbclient  
- Hydra  
- Searchsploit  
- Netcat  
- LinPEAS  

---

## 4) Approach (high level)

- Performed service enumeration to map the attack surface  
- Identified SMB as a key entry point and accessed anonymous shares  
- Extracted potential credentials and performed credential spraying on webmail  
- Used valid credentials to pivot between services and retrieve additional secrets  
- Discovered hidden web application and identified CMS  
- Exploited a Remote File Inclusion vulnerability to gain initial access  
- Performed local enumeration for privilege escalation vectors  
- Exploited a cron job using wildcard injection to escalate privileges  

---

## 5) Results / Evidence (sanitized)

- Anonymous SMB share exposed sensitive internal files including password lists  
- Successful credential reuse allowed access to webmail and additional SMB share  
- Discovered hidden CMS path leading to exploitable functionality  
- Achieved remote code execution via RFI vulnerability  
- Gained shell access as a low-privileged user  
- Privilege escalation achieved through tar wildcard injection in a cron job  
- Final state: full system compromise with root-level access  

---

## 6) Recommended remediation

- Disable anonymous access to SMB shares and enforce strict access controls  
- Apply strong password policies and prevent credential reuse across services  
- Secure web applications by validating and sanitizing user input  
- Prevent Remote File Inclusion by restricting external file loading  
- Use least privilege principles for services and scripts  
- Avoid unsafe wildcard usage in privileged scripts  
- Implement proper monitoring and auditing of cron jobs  

---

## 7) Lessons learned

- Enumeration is critical; small findings can chain into full compromise  
- SMB shares are often high-value targets for information disclosure  
- Credential reuse is a major weakness in real-world environments  
- Pivoting between services is key in exploitation workflows  
- Understanding common vulnerabilities (like RFI) is essential  
- Misconfigured automation (cron jobs) can lead to privilege escalation  

---

## 8) Links / Resources

- Room / machine link: https://tryhackme.com/room/skynet  
- Useful docs: https://book.hacktricks.xyz/  
- Notion / detailed write-up (public): N/A  

---
