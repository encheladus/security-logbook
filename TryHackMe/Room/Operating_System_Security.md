# Operating System Security — TryHackMe (Computer Science)

**Date:** 2026-02-26  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room introduces the fundamentals of operating system (OS) security and demonstrates SSH authentication on Linux.  
The objective was to understand how operating systems enforce security through authentication, permissions, and core security principles.

---

## 2) Initial hypothesis

Before starting, I expected that most operating system vulnerabilities would stem from user misconfigurations rather than inherent flaws in the OS itself.  

I anticipated exploring:

- Authentication weaknesses (especially passwords)
- Permission misconfigurations
- How Linux enforces access control
- How SSH security depends on credential strength

---

## 3) Tools used

- Linux terminal  
- SSH client  

---

## 4) Approach (high level)

- Reviewed the role of an operating system as an intermediary between hardware and applications  
- Studied hardware components (CPU, RAM, storage) and how the OS manages them  
- Analyzed the CIA triad (Confidentiality, Integrity, Availability)  
- Explored common OS security weaknesses:
  - Weak authentication
  - Poor password hygiene
  - Misconfigured file permissions
  - Malware execution risks  
- Practiced SSH authentication on Linux  
- Reflected on how misconfigurations can lead to privilege escalation or full system compromise  

---

## 5) Results / Evidence (sanitized)

Key observations:

- Weak passwords significantly increase the risk of unauthorized access.
- Poorly configured file permissions can expose sensitive data or allow unintended modifications.
- Malware can impact:
  - Confidentiality (data theft)
  - Integrity (data modification)
  - Availability (ransomware encryption)
- SSH provides secure remote access, but security entirely depends on strong authentication practices.
- If OS-level controls fail, all applications and data become vulnerable.

Final state understanding: Operating system security directly enforces access control, resource management, and the protection of the CIA triad.

---

## 6) Recommended remediation

- Enforce strong, unique passwords
- Implement multi-factor authentication where possible
- Apply the principle of least privilege
- Regularly audit file permissions
- Restrict root and administrative access
- Harden SSH configuration (disable weak authentication methods)
- Keep systems updated with security patches

---

## 7) Lessons learned

- The operating system is the core security authority of any device.
- Most real-world compromises originate from weak credentials or misconfigurations.
- The CIA triad is a practical framework, not just theory.
- Authentication and authorization are distinct but equally critical.
- Understanding defensive mechanisms is essential before attempting offensive techniques.

---

## 8) Links / Resources

- Room: TryHackMe – Computer Science (Operating System Security)
- Useful documentation: Linux manual pages (man), SSH documentation
- Detailed notes: Personal Notion workspace (public version)
---