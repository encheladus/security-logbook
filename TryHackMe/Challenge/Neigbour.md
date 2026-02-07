# Neighbour — TryHackMe (IDOR)

**Date:** 2026-02-07  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Challenge TryHackMe focused on **Insecure Direct Object References (IDOR)**, a subset of **Broken Access Control**.  
The objective is to identify whether user-controlled parameters allow unauthorized access to other users’ data.

---

## 2) Initial hypothesis

Before interacting with the application, the following potential weaknesses were considered:

- Insecure Direct Object References (IDOR)
- Weak or missing authorization checks
- Misconfigured authentication logic
- Possible client-side issues (XSS)
- Backend trust in user-supplied identifiers

Given the nature of the challenge and the presence of account-based access, IDOR was considered the most likely vulnerability.

---

## 3) Tools used

- Web browser (manual testing)
- Nmap (service reconnaissance)
- Gobuster (directory enumeration)

No credentials, scripts, or sensitive options are documented.

---

## 4) Approach (high level)

- Recon the exposed services to understand the attack surface
- Enumerate accessible web directories
- Inspect page source and application logic
- Authenticate using provided guest credentials
- Analyze URL patterns and user-controlled parameters
- Attempt parameter manipulation to access unauthorized resources

The focus remained on **logic flaws** rather than exploitation complexity.

---

## 5) Results / Evidence (sanitized)

- The application exposes a profile endpoint using a user-controlled parameter:
  - `profile.php?user=<username>`
- No server-side authorization checks were enforced
- Modifying the parameter allowed access to another user’s profile
- Administrative data was accessible from a low-privileged account

**Impact:**  
Unauthorized access to sensitive user data via IDOR, resulting in full profile disclosure.

---

## 6) Recommended remediation

- Enforce **server-side authorization checks** on every request
- Never trust user-supplied identifiers to reference sensitive objects
- Bind user identity strictly to the authenticated session
- Replace direct object references with indirect identifiers (UUIDs)
- Implement proper **role-based access control (RBAC)**
- Remove sensitive comments from client-side source code

---

## 7) Lessons learned

- IDOR vulnerabilities are often trivial but highly impactful
- Authentication does not imply authorization
- Parameter manipulation should always be tested
- Backend logic must never rely on client-side trust
- Developer comments can unintentionally expose attack paths

---

## 8) Links / Resources

- TryHackMe Room: Neighbour (IDOR)
- OWASP Top 10 — Broken Access Control
- OWASP IDOR Cheat Sheet