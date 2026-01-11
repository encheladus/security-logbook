# Extract — TryHackMe - Web Application Red Teaming

**Date:** 2026-01-11  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Short summary (1–3 lines):

This lab focuses on advanced web exploitation through vulnerability chaining.  
The objective was to extract two secrets by abusing a vulnerable document preview feature, internal services exposure, and weak authentication logic.

---

## 2) Initial hypothesis

Before starting, I expected a classic web exploitation scenario involving:
- Enumeration of exposed services
- Identification of a primary web vulnerability
- Chaining multiple weaknesses to reach protected internal functionality

Given the challenge description, I anticipated misconfigurations left behind during rushed development changes.

---

## 3) Tools used

- Nmap  
- Gobuster  
- ffuf  
- Python (custom proxy tooling)  
- Browser developer tools  
- Burp Suite (limited use)

---

## 4) Approach (high level)

1. Enumerated exposed services and identified a web application as the primary attack surface  
2. Analyzed client-side logic and identified a server-side request feature  
3. Confirmed and escalated an SSRF vulnerability  
4. Abused protocol handling to reach internal-only services  
5. Identified an internal Next.js application and bypassed middleware authentication  
6. Leveraged internal access to bypass IP-based restrictions  
7. Manipulated insecure client-controlled authentication data to bypass 2FA  

---

## 5) Results / Evidence (sanitized)

- Confirmed **SSRF** allowing arbitrary outbound requests
- Escalated SSRF using **protocol abuse** to interact with internal services
- Discovered an internal web service bound to localhost
- Bypassed **Next.js middleware authentication** via a known framework vulnerability
- Accessed protected internal APIs
- Bypassed **IP-based access control**
- Bypassed **2FA** by manipulating an insecure serialized authentication cookie
- Successfully extracted **two secrets (flags)**

Final state:  
- Internal API and management interfaces fully compromised from external access

---

## 6) Recommended remediation

- Enforce strict SSRF protections (allowlists, protocol restrictions)
- Disable dangerous schemes such as `gopher://`
- Apply proper network segmentation between frontend and internal services
- Do not rely on IP-based trust for authentication
- Secure Next.js middleware and keep frameworks up to date
- Never store authentication state in client-controlled serialized objects
- Implement signed, integrity-checked cookies
- Enforce server-side 2FA validation

---

## 7) Lessons learned

- SSRF vulnerabilities can escalate far beyond simple data retrieval
- Protocol abuse is a critical but often overlooked attack vector
- Internal services frequently over-trust localhost traffic
- Modern web exploitation often requires custom tooling
- Authentication logic must never rely solely on client-controlled data
- Effective exploitation is often about **chaining small weaknesses**

---

## 8) Links / Resources

- TryHackMe — Extract (Web Application Red Teaming)  
- Next.js Middleware Authentication Bypass (CVE‑2025‑29927)  
- SSRF & Gopher protocol abuse references  
- Public walkthrough (used as learning reference):  
  https://medium.com/@sornphut/extract-tryhackme-walkthough-881daf0ca120

---
