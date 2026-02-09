# Corridor — TryHackMe

**Date:** 2026-02-08  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Write-up of the **Corridor** room on TryHackMe.  
The lab focuses on identifying and exploiting a **Broken Access Control / IDOR** vulnerability in a web application that relies on hashed object identifiers.

Learning objective: recognize why **obfuscation (hashing)** does not equal **authorization**, and how simple logic flaws lead to unauthorized access.

---

## 2) Initial hypothesis

Before starting the lab, the following assumptions were made:

- The challenge is clearly **web application–based**
- The mention of **hexadecimal values** suggests encoded or hashed identifiers
- The web interface is the primary attack surface, making network enumeration unnecessary
- Despite its apparent simplicity, overconfidence is avoided

Initial expectation: a **Direct Object Reference** vulnerability hidden behind superficial obfuscation.

---

## 3) Tools used

- Web browser (manual testing)
- Hash analysis via online rainbow tables (e.g. CrackStation)

No automated exploitation tools or sensitive scripts were required.

---

## 4) Approach (high level)

The investigation followed a logical workflow:

- Explore the web application and observe navigation behavior
- Inspect URL parameters controlling page access
- Identify hash-like values used as object references
- Analyze whether hashing provides real protection or simple obfuscation
- Test decoded identifiers and boundary values outside the visible range
- Attempt access to unlisted or unintended resources

The focus was on **logic testing**, not brute force or automation.

---

## 5) Results / Evidence (sanitized)

Observed behavior:

- Page access is entirely controlled by a URL parameter
- The parameter value is an **MD5 hash** of a predictable numeric identifier
- Decoding the hash reveals simple sequential page numbers
- Modifying the identifier grants access to a hidden page not exposed in the UI
- The hidden page directly reveals the flag

Final impact: **unauthorized access to restricted content** through identifier manipulation.

---

## 6) Recommended remediation

To prevent this class of vulnerability:

- Enforce **server-side authorization checks** for every object request
- Validate object access against the authenticated user context
- Avoid relying on hashed or obfuscated identifiers for security
- Implement access control middleware consistently
- Use indirect references combined with authorization logic
- Apply defense-in-depth (logging, monitoring, least privilege)

Hashing identifiers alone provides **no security benefit**.

---

## 7) Lessons learned

- Obfuscation is not access control
- Hashing predictable values does not prevent IDOR
- URL parameters must always be treated as attacker-controlled
- IDOR vulnerabilities often require **reasoning**, not tooling
- Testing out-of-scope and boundary values is critical

This room reinforces how common and realistic IDOR issues are in real-world applications.

---

## 8) Links / Resources

- TryHackMe Room: Corridor  
- OWASP Top 10 – Broken Access Control  
- Public notes / write-up (if applicable)

---