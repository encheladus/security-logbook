# Atlassian CVE-2022-26134 — TryHackMe - Web App CVE

**Date:** 2026-01-21  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Interactive TryHackMe lab demonstrating an unauthenticated Remote Code Execution vulnerability affecting Atlassian Confluence Server and Data Center via OGNL injection (CVE-2022-26134).

Learning objective: understand how expression language evaluation flaws can lead to full system compromise before authentication.

---

## 2) Initial hypothesis

The application likely evaluates user-controlled input using OGNL before authentication or access control checks, allowing crafted requests to reach sensitive execution paths and trigger arbitrary code execution.

---

## 3) Tools used

- Web browser  
- HTTP interception proxy  
- curl  
- Public vulnerability documentation  

(No credentials, secrets, or sensitive scripts used.)

---

## 4) Approach (high level)

- Identify the affected Confluence versions and vulnerable execution flow  
- Study how OGNL expressions are processed in request handling  
- Analyze how user input reaches expression evaluation logic  
- Observe where authentication checks occur in the request lifecycle  
- Verify whether execution happens prior to authorization enforcement  

---

## 5) Results / Evidence (sanitized)

- Unauthenticated HTTP requests trigger OGNL expression evaluation  
- Arbitrary system commands are executed on the server  
- Command output is reflected in HTTP response headers  
- No authentication or user interaction is required  

Final state: remote command execution confirmed on a vulnerable instance.

---

## 6) Recommended remediation

- Upgrade Confluence to a patched version immediately  
- Disable or restrict OGNL expression evaluation on user input  
- Enforce strict input validation and expression allow‑listing  
- Apply Web Application Firewall rules blocking OGNL patterns  
- Monitor application and server logs for suspicious requests  

---

## 7) Lessons learned

- Expression languages are high‑risk attack surfaces  
- Authentication is irrelevant if execution happens before access control  
- URL paths can be as dangerous as parameters or request bodies  
- RCE does not require file uploads or interactive shells  
- HTTP headers can be abused as covert data exfiltration channels  

---

## 8) Links / Resources

- TryHackMe room: Web App CVE – Atlassian Confluence  
- Atlassian Security Advisory (CVE-2022-26134)  
- Volexity research on active exploitation  
- Public OGNL documentation  
