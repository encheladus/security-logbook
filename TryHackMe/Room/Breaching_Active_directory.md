# Breaching Active Directory — TryHackMe

**Date:** 2025-12-12  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context

This network covers techniques and tools used to acquire the **first set of AD credentials**, which can then be leveraged to enumerate and explore Active Directory. The focus is on understanding how credentials are often exposed through **infrastructure and configuration weaknesses**, not malware.

---

## 2) Initial hypothesis

Focuses on common ways attackers obtain **initial AD credentials**, frequently observed in real-world intrusions and security assessments.  

Key conceptual methods:

1. **NTLM Authenticated Services** – Legacy authentication mechanisms can unintentionally expose credentials.  
2. **LDAP Bind Credentials** – Applications/services may store or transmit directory credentials insecurely.  
3. **Authentication Relays** – Exploitable if signing or channel binding is not enforced.  
4. **Microsoft Deployment Toolkit (MDT) Configuration Files** – Embedded credentials intended for automation may be exposed.  

---

## 3) Tools used

- Burp Suite  
- Repeater  
- Turbo Intruder  
- curl  
- Responder  
- tcpdump  
- ldap-utils / slapd  
- sqlitebrowser  

(*Note: options, credentials, or sensitive scripts are excluded.*)

---

## 4) Approach (high level)

1. Analyze common **credential sources** (user-driven, service-driven, infrastructure-driven).  
2. Identify opportunities to **capture or validate credentials** without administrative access.  
3. Explore attack paths for each source:  
   - **OSINT & Phishing**: discover usernames/emails, test password reuse.  
   - **NTLM Services**: validate credentials, password spray.  
   - **LDAP**: identify bind accounts, capture via rogue server.  
   - **MDT / PXE**: retrieve boot artifacts, parse configuration for credentials.  
   - **Configuration Files**: inspect local databases or application configs for stored credentials.  
4. Treat each credential as a **foothold**, not a final goal.  
5. Document impact and potential access without exposing sensitive data.  

---

## 5) Results / Evidence (sanitized)

- **Initial AD credentials obtained** via:  
  - NTLM password spraying against exposed services  
  - Captured LDAP bind credentials through rogue LDAP server  
  - Extracted PXE/MDT boot configuration files containing domain accounts  
  - Decrypted service account passwords from endpoint configuration databases  

- Even low-privileged credentials enabled:  
  - Active Directory enumeration  
  - Misconfiguration identification  
  - Potential lateral movement and privilege escalation  

- Observed recurring pattern: credentials often leaked quietly **without touching a Domain Controller**.  

---

## 6) Recommended remediation

- Enforce **least-privilege accounts** for services and deployments.  
- Disable or reduce **NTLM usage**; enforce **SMB signing**.  
- Regularly rotate **LDAP bind accounts** and avoid storing passwords in plaintext.  
- Audit PXE/MDT images and deployment files for embedded credentials.  
- Remove or encrypt credentials in local configuration files and databases.  
- Monitor internal authentication for unusual patterns.  
- Educate employees on phishing risks and credential hygiene.  

---

## 7) Lessons learned

- Initial AD access is mostly due to **credential exposure**, not software exploits.  
- NTLM services act as **authentication oracles**; password spraying is safer than brute-force.  
- LDAP bind accounts and MDT service accounts can provide broad access.  
- Authentication relays exploit **protocol trust**, not weak passwords.  
- Configuration files often contain **encrypted but recoverable credentials**.  
- Even low-privileged credentials drastically expand attack surface.  
- Understanding *why* a technique works is more valuable than memorizing commands.  

---

## 8) Links / Resources

- [TryHackMe Room: Red Team](https://tryhackme.com/room/redteam)  
- [HaveIBeenPwned](https://haveibeenpwned.com/) – OSINT tool  
- [DeHashed](https://www.dehashed.com/) – credential search  
- Public Notion / detailed write-up: [link if available]  

---
