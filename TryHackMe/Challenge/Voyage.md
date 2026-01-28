# Voyage — TryHackMe

**Date:** 2026-01-28  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Multi-stage web and infrastructure exploitation challenge focused on distinguishing containerized “root” access from real host compromise.  
Objective: chain web vulnerabilities, internal trust failures, and container misconfiguration to achieve full system control.

---

## 2) Initial hypothesis

The challenge likely requires chaining multiple weaknesses rather than relying on a single exploit.  
Early “root” access may be deceptive, suggesting containerization and the need for pivoting or escape techniques.

---

## 3) Tools used

- Nmap  
- Curl  
- SSH client  
- Python (analysis & payload crafting)  
- Netcat  
- Linux enumeration utilities  

*(No sensitive scripts or credentials included.)*

---

## 4) Approach (high level)

1. Enumerated exposed services to identify external attack surface  
2. Identified CMS in use and researched known authentication bypass issues  
3. Leveraged misconfigured API endpoint to retrieve application secrets  
4. Used credential reuse to obtain shell access  
5. Identified containerized environment and enumerated internal network  
6. Discovered and accessed an internal-only web application  
7. Analyzed session handling and identified insecure deserialization  
8. Achieved code execution within the container  
9. Enumerated container capabilities and kernel exposure  
10. Abused excessive capabilities to escape container isolation  
11. Verified host-level control and retrieved final artifacts  

---

## 5) Results / Evidence (sanitized)

- Unauthenticated access to sensitive application configuration data  
- Successful SSH access as root inside a containerized environment  
- Internal service accessible only from the container network  
- Authentication bypass due to missing validation logic  
- Arbitrary code execution via unsafe Python pickle deserialization  
- Container granted `CAP_SYS_MODULE`, allowing kernel module insertion  
- Kernel-level execution achieved without exploiting a kernel CVE  
- Full host compromise confirmed  

*(Evidence limited to behavioral outcomes and final system state.)*

---

## 6) Recommended remediation

- Enforce authentication and authorization on all API endpoints  
- Never expose internal services without access controls  
- Replace unsafe deserialization (e.g., pickle) with secure formats  
- Implement defense-in-depth for containerized workloads:
  - Drop unnecessary Linux capabilities
  - Disable kernel module loading
  - Remove kernel headers from containers
  - Apply seccomp and AppArmor profiles
- Treat internal networks as untrusted  
- Enforce strict separation between application and host privileges  

---

## 7) Lessons learned

- Root access does not always imply host compromise  
- Container security depends more on capabilities than user ID  
- Misconfiguration alone can lead to kernel-level execution  
- Insecure deserialization is equivalent to arbitrary code execution  
- Internal services are often the weakest link in real-world systems  

---

## 8) Links / Resources

- TryHackMe – Voyage room  
- Joomla CVE‑2023‑23752 documentation  
- OWASP: Insecure Deserialization  
- Docker & Linux Capabilities hardening guides  
- Public Notion write-up (sanitized)