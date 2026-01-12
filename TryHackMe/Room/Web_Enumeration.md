# Web Enumeration — TryHackMe - Web Application

**Date:** 2026-12-12  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

This lab focuses on foundational **web enumeration methodology**, introducing both manual and automated techniques.  
The objective is to learn how to effectively map a web application’s attack surface while minimizing noise and false positives.

---

## 2) Initial hypothesis

The target would expose discoverable structure and metadata through basic enumeration techniques.  
Manual inspection was expected to reveal indicators (technology, CMS, assets) that would guide the selection and configuration of automated tools.

---

## 3) Tools used

- Web browser (manual inspection, Developer Tools)
- Gobuster
- WPScan
- Nikto

*(No credentials, exploits, or sensitive scripts used)*

---

## 4) Approach (high level)

1. Performed **manual enumeration first** using the browser to identify technologies, assets, and structure.
2. Used Developer Tools to inspect source code, loaded resources, and application behavior.
3. Applied **Gobuster** for structured discovery (directories, files, subdomains, virtual hosts).
4. Identified the application as **WordPress**, then shifted to **WPScan** for CMS-specific enumeration.
5. Used **Nikto** selectively to assess server-level misconfigurations and common exposures.
6. Iteratively refined scans based on findings to reduce noise and improve relevance.

---

## 5) Results / Evidence (sanitized)

- Discovered hidden paths and application structure through directory enumeration.
- Identified WordPress-specific indicators (themes, plugins, users).
- Observed server-level misconfigurations and informational disclosures.
- Confirmed that aggressive, untuned scans produced excessive false positives.
- Final state: **attack surface successfully mapped without exploitation**.

---

## 6) Recommended remediation

- Enforce proper file and directory permissions.
- Disable directory listing where unnecessary.
- Remove or restrict access to sensitive default files.
- Implement WAF rules and rate limiting for enumeration-heavy endpoints.
- Keep CMS core, plugins, and themes fully updated.
- Minimize information leakage via headers, comments, and metadata.

---

## 7) Lessons learned

- Manual enumeration is essential and often faster than automation.
- Automated tools must be **context-driven**, not run blindly.
- Enumeration is iterative and hypothesis-driven.
- Noise management is a core skill in web testing.
- Correctly identifying the underlying technology early drastically improves results.

---

## 8) Links / Resources

- TryHackMe — Web Enumeration room  
- Gobuster documentation  
- WPScan documentation  
- Nikto documentation  
- Public Notion write-up (if applicable)

---
