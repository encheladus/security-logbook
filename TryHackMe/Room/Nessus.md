# Nessus - TryHackMe - Pentester Tools

**Date:** 2025-12-14  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

TryHackMe *Pentester Tools* classroom focused on **Nessus Essentials**.  
Objective: understand how vulnerability scanning works, how Nessus detects issues, and how to correctly interpret scan results instead of treating them as ground truth.

---

## 2) Initial hypothesis

Before starting, I expected Nessus to act as a largely automated vulnerability finder:  
- Point it at a target  
- Run a scan  
- Receive a reliable list of exploitable vulnerabilities  

The underlying assumption was that high vulnerability counts directly correlate with high real-world risk.

---

## 3) Tools used

- Nessus Essentials (local web interface)
- Web browser (HTTPS access to scanner UI)

---

## 4) Approach (high level)

- Set up and access Nessus Essentials via its local web interface  
- Explore the dashboard, scans, policies, and plugin structure  
- Create and run a **Basic Network Scan** against an authorized lab target  
- Observe scan workflow: discovery → port scanning → service enumeration → vulnerability correlation  
- Review results by severity, host, and plugin  
- Analyze informational findings before high/critical issues  
- Compare unauthenticated vs authenticated scan capabilities conceptually  
- Reflect on scan accuracy, false positives, and scanner limitations  

---

## 5) Results / Evidence (sanitized)

- Nessus successfully identified:
  - Live hosts
  - Open ports and exposed services
  - Service versions and banners
- Vulnerabilities were reported with:
  - Severity levels (Critical → Informational)
  - CVSS scores
  - Affected services and remediation guidance
- Observed that:
  - Informational findings often provided the most useful reconnaissance data
  - High vulnerability counts did not necessarily imply high exploitability
  - Host discovery settings could cause entire systems to be skipped
- No exploitation or credential abuse was performed (detection-only scan)

---

## 6) Recommended remediation

General defensive measures highlighted by the scanner and analysis:

- Keep systems and services patched and up to date
- Restrict unnecessary exposed services and open ports
- Harden configurations (authentication, encryption, defaults)
- Use authenticated scans for accurate internal visibility
- Validate scanner findings manually to eliminate false positives
- Apply proper scoping and authorization for all scans

---

## 7) Lessons learned

- Nessus does not *find* vulnerabilities — it **correlates observed conditions with known weakness patterns**
- Vulnerability scanning is an extension of enumeration, not a replacement for it
- Host discovery settings are critical and can invalidate scan results
- Unauthenticated scans reflect an external attacker’s perspective only
- Authenticated scans dramatically improve depth and accuracy
- Informational findings are essential for attack surface mapping
- CVSS scores help prioritize but lack contextual awareness
- Nessus complements Nmap:
  - Nmap shows *what exists*
  - Nessus suggests *what might be wrong*

---

## 8) Links / Resources

- TryHackMe — Pentester Tools (Nessus section)
- Tenable Nessus Documentation
- Public write-up (Notion → GitHub)

---