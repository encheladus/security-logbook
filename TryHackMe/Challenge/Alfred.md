# Alfred — TryHackMe

**Date:** 2026-03-15  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context
This lab focuses on exploiting a misconfigured Jenkins instance to gain initial access, followed by privilege escalation on a Windows system via token impersonation.

## 2) Initial hypothesis
- Jenkins may be accessible with default or weak credentials  
- Misconfigured CI/CD service could allow command execution  
- Windows privileges might enable token impersonation for escalation  

## 3) Tools used
- Nmap  
- Jenkins  
- PowerShell (Nishang)  
- Metasploit (Meterpreter, Incognito)  

## 4) Approach (high level)
- Enumerate open ports and identify exposed services  
- Discover Jenkins interface and attempt default credentials  
- Use Jenkins job execution to achieve remote command execution  
- Establish a reverse shell via PowerShell  
- Upgrade shell to Meterpreter for stability  
- Enumerate privileges and identify impersonation opportunities  
- Abuse token impersonation to escalate privileges  
- Migrate process to obtain stable SYSTEM-level access  

## 5) Results / Evidence (sanitized)
- Successful authentication to Jenkins using default credentials  
- Remote command execution achieved through Jenkins jobs  
- Reverse shell obtained as low-privileged user  
- Privilege escalation to SYSTEM via token impersonation  
- Final state: full administrative access to the target system  

## 6) Recommended remediation
- Disable or change default credentials on all services  
- Restrict access to Jenkins and similar administrative interfaces  
- Implement strong authentication mechanisms  
- Limit privileges assigned to service accounts  
- Monitor and restrict use of token impersonation privileges  
- Apply principle of least privilege across the system  

## 7) Lessons learned
- Default credentials remain a critical and common attack vector  
- CI/CD tools like Jenkins can expose powerful execution capabilities if misconfigured  
- Stabilizing shells significantly improves post-exploitation workflow  
- Windows privileges such as SeImpersonatePrivilege are key for escalation  
- Understanding token behavior is essential for reliable privilege escalation  
- Process migration is required to fully leverage elevated privileges  
- A structured methodology (enumeration → exploitation → escalation) is essential  

## 8) Links / Resources
- TryHackMe Alfred room  
- Nishang framework  
- Metasploit documentation  
- Windows token impersonation documentation  

---  
