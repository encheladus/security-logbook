# [OWASP API Security Top 10 - 1] — TryHackMe - Web Fundamentals  
**Date:** 2025-10-13  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context  
Intro to secure API design and testing: core concepts (what an API is and why it matters), OWASP-style API risks, and hands-on checks for common API vulnerabilities such as Broken Object Level Authorization (BOLA / IDOR), Broken Function Level Authorization (BFLA), Excessive Data Exposure, authentication problems, and rate/resource controls.

## 2) Initial hypothesis  
APIs are “data servers,” but attackers can manipulate requests or exploit poor server-side controls. The lab should reveal how simple request tampering or missing checks leads to data leaks, account takeover, or privileged actions — and teach server-side mitigations and test techniques.

## 3) Tools used  
Talend API Tester and Laravel-based web.  

## 4) Approach (high level)  
- Learn API fundamentals and inspect endpoints via docs / discovery.  
- Test authentication (AuthN) and token handling.  
- Test object-level access (BOLA/IDOR): try accessing other IDs with a low-privilege token.  
- Test function-level access (BFLA): call admin endpoints with non-admin tokens.  
- Inspect API responses for excessive fields and sensitive values.  
- Run lightweight rate-limit/resource checks.  
- Recommend server-side fixes and monitoring; iterate tests after mitigations.

## 5) Results / Evidence (sanitized)  
- Demonstrated how substituting object IDs can return other users’ data when object-level checks are missing.  
- Showed that administrative endpoints can be invoked by normal users if function-level checks are absent.  
- Found examples where API responses included sensitive internal fields (password hashes, role flags) that the UI didn’t display.  
- Performed a small burst test to detect absence of rate limiting; observed HTTP codes to infer throttling behavior.  
- Verified JWT structure and claims via local decode (no secret disclosure).

## 6) Recommended remediation  
- **AuthN/AuthZ:** Use proven libraries for authentication; validate tokens, rotate/expire them, use HttpOnly/Secure cookies where appropriate. Centralize and enforce authorization checks per endpoint and per object (deny-by-default).  
- **BOLA/BFLA fixes:** Enforce object ownership checks server-side; implement role/attribute-based guards for function-level actions; avoid client-side-only enforcement.  
- **Data minimization:** Explicit serializers/DTOs or allow-lists for response fields; never return secrets or internal flags by default.  
- **Rate & resource controls:** Implement per-client quotas, throttling (429 responses), and max payload sizes. Use caching and queueing for expensive ops.  
- **Logging & detection:** Log user-id + object-id + endpoint access; alert on enumeration patterns or anomalous rates.  
- **Dev hygiene:** API docs should be accurate; run automated and manual tests as part of CI; include response-audit checks in QA.

## 7) Lessons learned  
- Authenticated ≠ authorized: always test both object-level and function-level authorization.  
- UIs hide fields — always inspect raw API responses.  
- Rate limiting is a security control as well as a performance control.  
- Centralized, deny-by-default server-side authorization is the most reliable defense.  
- Production telemetry (logs, rate metrics) is essential to detect enumeration and automated abuse.

## 8) Links / Resources  
- OWASP API Security Top 10 (2019) — guidance & examples.  
- OWASP cheat-sheets: Authentication, Access Control, Rate Limiting.

---