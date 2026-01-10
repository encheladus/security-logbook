# Farewell Challenge — TryHackMe - Web Application Red Teaming

**Date:** 2026-01-10  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Web application red-teaming lab focused on **WAF evasion**, **authentication weaknesses**, and **stored XSS leading to privilege escalation**.  
Objective: bypass defensive controls to obtain **administrative access** and read restricted content.

---

## 2) Initial hypothesis

- Limited exposed services → attack surface likely web-only
- Presence of a WAF → classic payloads probably blocked
- Potential weaknesses:
  - Verbose authentication errors
  - Weak rate limiting
  - Trust in client-controlled headers
  - Stored user content reviewed by an administrator

---

## 3) Tools used

- Network scanning tools  
- Directory enumeration tools  
- Burp Suite (proxy & request analysis)  
- Fuzzing tools  
- Browser developer tools  

*(No sensitive scripts, payloads, or credentials included)*

---

## 4) Approach (high level)

1. **Reconnaissance**
   - Identify exposed services and web endpoints
   - Enumerate accessible and restricted resources
   - Review publicly accessible diagnostic pages

2. **Authentication analysis**
   - Test login behavior for inconsistencies
   - Observe differences in server responses
   - Identify potential username disclosure

3. **WAF behavior analysis**
   - Trigger and observe filtering patterns
   - Identify rate-limiting thresholds
   - Test trust boundaries (e.g., client-supplied headers)

4. **Credential compromise**
   - Derive constrained password patterns from contextual hints
   - Bypass rate limiting via logic flaws
   - Obtain valid user access

5. **Privilege escalation**
   - Identify stored user input reviewed by admin
   - Craft obfuscated payloads to bypass WAF inspection
   - Capture administrative session context
   - Replay session to access restricted panel

---

## 5) Results / Evidence (sanitized)

- Valid usernames disclosed through verbose error messages
- Rate limiting enforced per IP but bypassable via untrusted headers
- Successful authentication as a normal user
- Stored XSS executed in an administrative context
- Administrative session hijacked
- Full access to admin panel and restricted data obtained

*(Evidence limited to behavioral outcomes — no secrets, cookies, or dumps)*

---

## 6) Recommended remediation

- Normalize authentication error messages
- Enforce server-side rate limiting independent of client headers
- Do not trust `X-Forwarded-For` or similar headers without validation
- Implement proper output encoding for all stored user content
- Harden WAF rules and keep CRS signatures up to date
- Use HTTP-only, secure cookies with proper session binding
- Apply defense-in-depth (WAF ≠ primary security control)

---

## 7) Lessons learned

- WAFs are **bypassable**, especially when relying on signatures
- Small logic flaws can enable **critical attack chains**
- Username enumeration is often overlooked but highly impactful
- Stored XSS remains one of the most powerful escalation vectors
- Modern attacks succeed through **chaining**, not single exploits

---

## 8) Links / Resources

- TryHackMe — Farewell (Web Application Red Teaming)
- OWASP Top 10 (Injection, XSS, Auth Failures)
- OWASP WAF Evasion & CRS documentation
- Public portfolio write-up (if applicable)

---
