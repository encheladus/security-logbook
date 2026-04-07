# Relevant — TryHackMe

**Date:** 2026-04-04  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

This lab was a Windows-based penetration testing challenge focused on chaining common misconfigurations into full compromise. The objective was to move from external enumeration to initial access, then escalate privileges and document the weaknesses that made the attack path possible.

## 2) Initial hypothesis

Before starting, I expected a realistic black-box path where exposed network services would reveal a practical entry point. Based on the target profile, SMB and HTTP looked like the most promising surfaces, with the possibility that a misconfiguration or weak operational practice would allow access that could later be escalated.

## 3) Tools used

- Nmap
- smbclient
- smbmap
- curl
- xfreerdp
- netcat
- msfvenom
- certutil
- PrintSpoofer

## 4) Approach (high level)

I began with service enumeration to identify exposed protocols and understand the attack surface. SMB stood out because guest access was possible and a non-standard share was exposed. That share contained encoded credentials, which were decoded and reused against other accessible services.

After confirming that one of the recovered accounts had write access to the SMB share, I tested whether uploaded content could also be reached through the web server. Once that mapping was confirmed, I used it as a route to obtain remote code execution through IIS.

From the low-privileged shell, I performed local privilege enumeration and identified a dangerous Windows token privilege. That finding led to a privilege escalation path that resulted in SYSTEM-level access and full compromise of the machine.

## 5) Results / Evidence (sanitized)

The assessment resulted in full compromise of the target through a multi-step attack chain.

The main observations were:

- Anonymous access was allowed to a custom SMB share
- The share exposed a file containing reversibly encoded credentials
- One recovered account was valid for SMB and had write access to the share
- The writable share was mapped to a web-accessible IIS directory
- Uploaded ASPX content could be executed through the web server
- The compromised context had `SeImpersonatePrivilege`
- Local privilege escalation to SYSTEM was successful

Sanitized impact evidence:

- Initial remote code execution was obtained through the IIS-exposed writable share
- User-level access was confirmed by reading the user flag
- SYSTEM-level access was confirmed after privilege escalation
- Final state: full host compromise from unauthenticated SMB exposure to administrative control

## 6) Recommended remediation

- Disable anonymous or guest access to SMB shares unless there is a strict business requirement
- Remove sensitive files from shared directories, especially credential-related material
- Never store passwords in reversible encodings such as Base64
- Apply least-privilege permissions on SMB shares and remove unnecessary write access
- Ensure writable shares are never mapped to web-accessible directories
- Restrict what file types can be executed by the web server
- Review IIS configuration and isolate content upload locations from executable application paths
- Audit service accounts and application pool identities for dangerous privileges
- Remove or tightly control `SeImpersonatePrivilege` where it is not explicitly required
- Monitor for suspicious file uploads, token impersonation abuse, and unexpected process creation on Windows servers

## 7) Lessons learned

- A single low-severity misconfiguration can become critical when chained with other weaknesses
- Base64 is only encoding, not protection
- Custom SMB shares deserve immediate attention during enumeration
- Credential reuse across services is always worth testing
- Writable locations become far more dangerous when they intersect with another exposed service
- On Windows, local privilege enumeration is essential as soon as a shell is obtained
- Token privileges can matter more than the apparent account name or role
- Clean exploitation often comes from validating each assumption step by step instead of guessing

## 8) Links / Resources

- Room / machine link: TryHackMe — Relevant
- Useful docs: Microsoft IIS documentation, SMB security hardening guidance, Windows privilege escalation references for impersonation privileges
- Notion / detailed write-up (public): Personal notes based on this room

---
