# HackPark — TryHackMe

**Date:** 2026-03-18  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Short summary: This lab focuses on brute-forcing a web login, exploiting a known vulnerability in a CMS to gain remote code execution, and performing privilege escalation on a Windows system.

---

## 2) Initial hypothesis

The target likely contains weak authentication mechanisms vulnerable to brute-force attacks, followed by a known public exploit allowing code execution. Privilege escalation may rely on misconfigured services or outdated system components.

---

## 3) Tools used

- Hydra  
- Metasploit  
- msfvenom  
- certutil  
- winPEAS  

---

## 4) Approach (high level)

- Identify a valid username and perform a brute-force attack against the login form  
- Gain access to the admin panel using discovered credentials  
- Identify the application version and search for known vulnerabilities  
- Adapt and use a public exploit to achieve remote code execution  
- Establish a reverse shell on the target system  
- Perform system enumeration to identify privilege escalation vectors  
- Abuse misconfigured scheduled tasks or services to escalate privileges  
- Alternatively, enumerate the system manually to identify missing patches and potential vulnerabilities  

---

## 5) Results / Evidence (sanitized)

- Successful authentication via brute-force attack  
- Remote code execution achieved through a vulnerable CMS component  
- Initial access obtained with limited privileges  
- Privilege escalation achieved to administrative level  
- Final state: full system compromise with access to user and administrator-level resources  

---

## 6) Recommended remediation

- Implement account lockout and rate limiting to prevent brute-force attacks  
- Enforce strong password policies  
- Regularly update and patch web applications and underlying systems  
- Restrict access to administrative interfaces  
- Validate and sanitize file uploads to prevent malicious payload execution  
- Apply principle of least privilege for services and scheduled tasks  
- Monitor and audit system logs for suspicious activity  

---

## 7) Lessons learned

- Brute-force attacks remain highly effective against weak authentication mechanisms  
- Public exploits require understanding and adaptation to work reliably  
- Initial access is only a small part of the attack chain; enumeration is critical  
- Windows privilege escalation often relies on misconfigurations rather than complex exploits  
- Automated tools are useful, but manual analysis is essential to fully understand attack paths  

---

## 8) Links / Resources

- Room / machine link  
- Exploit-DB  
- winPEAS (PEASS-ng)  
- Notion / detailed write-up (public)  

---
