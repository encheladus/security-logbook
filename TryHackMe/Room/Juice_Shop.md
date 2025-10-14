# [Juice Shop] — TryHackMe - Web Fundamentals  
**Date:** 2025-10-14  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context  
Use OWASP Juice Shop to **bridge theory → practice** for common web vulns: injection, auth weaknesses, sensitive data exposure, broken access control, and XSS. Focus on spotting tell-tale behaviors, testing safely, and mapping fixes.

## 2) Initial hypothesis  
I know the OWASP Top 10 conceptually; I want to **recognize them in a live app**, understand the signals in requests/responses, and tie each finding to concrete mitigations.

## 3) Tools used  
Burp Suite (Proxy/Repeater/Intruder), OWASP ZAP (optional), browser DevTools, `curl`, `jq`.

## 4) Approach (high level)  
- **Recon**: map endpoints, parameters, roles, and response shapes.  
- **Probe safely**: boolean/time-based tests for injection; harmless XSS payloads; role/ID toggles for authz.  
- **Compare diffs**: same request with varied IDs/roles; observe status codes, timing, and body changes.  
- **Document**: full request/response pairs + why the behavior indicates a vuln.  
- **Remediation mapping**: attach least-privilege, parameterization, output encoding, and gateway controls to each class.

## 5) Results / Evidence (sanitized)  
- **Injection**: inputs that changed response shape/timing suggested concatenated queries; boolean/time probes produced distinct outcomes.  
- **Auth weaknesses**: differing error messages for unknown vs bad password; session flags/rotation verified via cookies/headers.  
- **Sensitive exposure**: browsable paths (`/backup`, `/uploads`) and verbose JSON hinted at excess fields.  
- **Broken access control**: low-priv user could query admin paths/other IDs conceptually (expected 403; any 200 would be a finding).  
- **XSS**: reflection contexts identified (HTML/attribute/DOM). Harmless payloads helped locate encoding gaps.

## 6) Recommended remediation  
- **Injection**: parameterized queries, strict input validation, avoid shelling out; centralized logging for patterns.  
- **Auth**: MFA, lockout + throttling, generic error messages, robust reset with time-bound single-use tokens; secure cookies (`HttpOnly`, `Secure`, `SameSite`).  
- **Exposure**: deny directory listing, remove secrets from webroots, explicit serializers/DTO allow-lists.  
- **Access control**: server-side, deny-by-default checks for **both** object-level (BOLA) and function-level (BFLA); central middleware; role audits.  
- **XSS**: context-aware output encoding, CSP (report-only → enforce), avoid injecting user data into JS; `X-Content-Type-Options: nosniff`.

## 7) Lessons learned  
- Most issues are **trust boundary** failures: the server trusts client input/identity too much.  
- UI ≠ source of truth—**inspect raw API/HTML**.  
- Rate limiting is a **security** control, not just performance tuning.  
- Layered defenses (param + encoding + authz + CSP) catch overlap between classes.

## 8) Links / Resources  
- OWASP Juice Shop (project & wiki)  
- OWASP Top 10 (Web + API)  
- OWASP Cheat Sheets: Injection, XSS, Authentication, Access Control, Rate Limiting  
- PortSwigger / Burp learning materials (labs on SQLi/XSS/BAC)

---