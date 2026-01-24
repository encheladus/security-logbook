# OWASP Broken Access Control — TryHackMe 

**Date:** 2026-01-23  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Short summary (1–3 lines):

This lab covers **Broken Access Control**, ranked #1 in the OWASP Top 10.  
The objective is to understand access control models, identify authorization flaws, and exploit a vertical privilege escalation in a controlled web application.

---

## 2) Initial hypothesis

Before starting the lab, the following assumptions were made:

- Authorization checks may rely on client-controlled values
- Privilege separation (user vs admin) might be enforced inconsistently
- Administrative functionality could be reachable via parameter manipulation or direct endpoint access

---

## 3) Tools used

- Burp Suite (Proxy, Repeater)
- Web browser
- TryHackMe lab environment

> No credentials, payloads, or sensitive scripts are documented.

---

## 4) Approach (high level)

- Analyze authentication and post-login workflow
- Intercept and inspect HTTP requests and responses
- Identify authorization-related parameters exposed to the client
- Test direct access to restricted endpoints
- Observe whether server-side authorization checks are enforced

---

## 5) Results / Evidence (sanitized)

Observed behavior:

- Post-authentication logic exposed a privilege-related flag via a client-visible redirect parameter
- Modifying this value allowed access to an administrative page
- No server-side authorization validation was performed
- Administrative functionality became accessible to a standard user

Final state (sanitized):

- Unauthorized access to admin interface confirmed
- Ability to modify user roles observed
- Vertical privilege escalation successfully demonstrated

---

## 6) Recommended remediation

Generic defensive measures:

- Enforce authorization checks **server-side on every request**
- Never trust client-controlled role or privilege indicators
- Implement centralized Role-Based Access Control (RBAC)
- Validate user permissions using trusted session or database values
- Remove authorization logic from redirects and client responses
- Regularly audit access control rules and business logic

---

## 7) Lessons learned

- Authentication does not imply authorization
- Client-side values must always be considered untrusted
- Access control failures are often logic flaws, not technical exploits
- Every sensitive endpoint must independently verify permissions
- Vertical privilege escalation can fully compromise an application

---

## 8) Links / Resources

- TryHackMe – OWASP Broken Access Control room  
- OWASP Top 10 – Broken Access Control documentation  
- Public write-up (sanitized)
