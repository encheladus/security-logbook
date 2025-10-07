# [Vulnerability Research — CVSS, VPR & Databases] — TryHackMe - Junior Pentester  
**Date:** 2025-10-07  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context  
Learn to define and categorize vulnerabilities, understand how professionals **score (CVSS)** and **prioritize (VPR)** them, and apply research methods using databases like **NVD** and **Exploit-DB**. The goal: bridge theory (scoring, risk) with practice (exploit availability, mitigation).

## 2) Initial hypothesis  
Vulnerabilities are weaknesses, but effective pentesters must also master how to **find**, **evaluate**, and **rank** them.  
Assumption: efficiency comes from knowing *where to search (NVD, Exploit-DB)* and *how to interpret severity (CVSS/VPR)* — connecting technical flaws to real-world exploitability.

## 3) Tools used  
NVD (National Vulnerability Database), Exploit-DB, MITRE CVE, Tenable resources, vendor advisories, Google search operators.

## 4) Approach (high level)  
- Reviewed definitions and **categories** of vulnerabilities (OS, config, logic, human).  
- Studied **CVSS**: standard, impact-driven scoring (static, theoretical).  
- Compared with **VPR**: dynamic, real-world risk prioritization.  
- Practiced **cross-database research** (CVE lookup → severity → exploit existence).  
- Built a **search and validation workflow** to evaluate vulnerability relevance.  
- Mapped findings back to pentest and remediation context.

## 5) Results / Evidence (sanitized)  
- Understood CVSS rating scale (0–10) and categories (None → Critical).  
- Learned VPR’s adaptive risk evaluation (based on 150+ factors).  
- Created actionable workflow for vulnerability research:
  1. Identify product/version.  
  2. Search NVD → confirm CVE and CVSS.  
  3. Search Exploit-DB → check PoC/exploit presence.  
  4. Cross-reference vendor advisories and security blogs.  
  5. Document CVE, PoC, and mitigation in structured notes.  
- Practiced keyword queries like `"nginx 1.20.1 CVE"` and `site:exploit-db.com`.

## 6) Recommended remediation  
- Maintain **up-to-date patching** policy — prioritize based on **risk (VPR)** not just **severity (CVSS)**.  
- Always **validate exploit availability** before assigning urgency.  
- Use **trusted databases** (NVD, MITRE, Exploit-DB) for triage, not random forums.  
- Automate **vulnerability monitoring** for critical software stacks.  
- Educate dev/ops teams on **proper configuration** and **secure design** to prevent misconfigurations.

## 7) Lessons learned  
- Vulnerability ≠ exploit — they’re related but distinct.  
- **CVSS** measures impact; **VPR** measures *real-world* likelihood.  
- Around **80% of CVEs lack exploits**, proving why prioritization matters.  
- **NVD = official data**, **Exploit-DB = practical PoCs** — both are complementary.  
- Responsible use of PoCs is essential — always test **only in authorized environments**.  
- Vulnerability research is part of both **reconnaissance** and **reporting** in pentesting.

## 8) Links / Resources  
- [NVD — National Vulnerability Database](https://nvd.nist.gov/vuln)  
- [Exploit-DB](https://www.exploit-db.com/)  
- [MITRE CVE](https://cve.mitre.org/)  
- [GitHub Security Advisories](https://github.com/advisories)  
- [OSV.dev](https://osv.dev/)  
- [Tenable Research](https://www.tenable.com/research)

---