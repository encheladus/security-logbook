# Game Zone — TryHackMe

**Date:** 2026-03-21  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context
Short summary (1–3 lines):  
Exploitation of a vulnerable web application involving SQL injection, credential cracking, SSH tunneling, and privilege escalation via a vulnerable Webmin service.

## 2) Initial hypothesis
The application likely contains a SQL injection vulnerability allowing authentication bypass and database extraction. Retrieved credentials could be cracked and reused for lateral movement. Internal services may be exposed via tunneling, leading to privilege escalation.

## 3) Tools used
SQLMap, John the Ripper, SSH, Metasploit, ss

## 4) Approach (high level)
Identify authentication bypass via SQL injection.  
Leverage automated tooling to extract database contents.  
Crack retrieved password hashes to gain valid user access.  
Enumerate internal services and expose them using SSH tunneling.  
Exploit a vulnerable service version to achieve remote command execution and escalate privileges.

## 5) Results / Evidence (sanitized)
Successful authentication bypass.  
Database fully dumped including user credentials.  
Password cracked and reused for valid system access.  
Internal service exposed and accessed via tunnel.  
Final state: full system compromise with root-level access.

## 6) Recommended remediation
Use prepared statements and parameterized queries to prevent SQL injection.  
Enforce strong password policies and secure hashing mechanisms (e.g., bcrypt).  
Restrict internal services and apply proper network segmentation.  
Keep services updated and patch known vulnerabilities.  
Limit access to administrative interfaces and enforce least privilege.

## 7) Lessons learned
- SQL injection can lead to full database compromise if not mitigated  
- Automation tools are powerful but require proper context and usage  
- Weak passwords significantly increase attack impact  
- SSH tunneling enables access to otherwise unreachable services  
- Chaining vulnerabilities is key to achieving full system compromise  

## 8) Links / Resources
- Room / machine link  
- Useful docs  
- Notion / detailed write-up (public)  

---
