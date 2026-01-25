# Lo-Fi — TryHackMe - LFI vulne

**Date:** 2026-01-25  
**Type:** Challenge  
**Scope:** Lab / Authorized only

---

## 1) Context

Short summary (1–3 lines):

This TryHackMe challenge focuses on identifying and exploiting a **Local File Inclusion (LFI)** vulnerability.  
The objective is to understand how dynamic file loading can be abused to access sensitive files and retrieve the flag.

---

## 2) Initial hypothesis

Before starting, the assumption was that the application might expose a common web vulnerability through user-controlled input.  
Initial expectations leaned toward client-side injection (e.g., XSS) or insecure parameter handling leading to file access.

---

## 3) Tools used

- Nmap  
- Gobuster  
- Burp Suite (Proxy & Repeater)  
- Web browser  

*(No sensitive scripts, credentials, or exploit code used.)*

---

## 4) Approach (high level)

- Performed basic network reconnaissance to identify exposed services  
- Enumerated web directories and application entry points  
- Manually reviewed application behavior and user-controlled parameters  
- Inspected HTTP traffic to identify dynamically loaded resources  
- Tested file inclusion logic at a conceptual level  
- Iteratively adjusted traversal depth to bypass naive filtering  

---

## 5) Results / Evidence (sanitized)

- Identified a **dynamic file inclusion parameter** used to load content pages  
- Direct access to sensitive system files was initially blocked by basic filtering  
- Using relative traversal techniques, arbitrary file reading became possible  
- Successfully confirmed LFI by accessing a standard system file  
- Applied the same technique to retrieve the challenge flag  

**Impact:**  
- Arbitrary file read on the server  
- Flag disclosure through unauthorized file access  

(No raw file dumps, tokens, or sensitive values included.)

---

## 6) Recommended remediation

- Avoid dynamic file inclusion based on user input  
- Enforce strict allowlists for includable resources  
- Normalize and validate file paths before processing  
- Implement realpath checks to prevent traversal  
- Use application-level access controls and error handling  

---

## 7) Lessons learned

- LFI vulnerabilities often hide behind legitimate parameters like `page` or `view`  
- Blacklist-based filtering is weak and easily bypassed  
- When common injection vectors fail, request inspection is critical  
- Understanding application logic is as important as automated enumeration  

---

## 8) Links / Resources

- TryHackMe room: *Lo-Fi*  
- OWASP: Local File Inclusion (LFI)  
- Public write-up (sanitized): *(to be added)*  

---