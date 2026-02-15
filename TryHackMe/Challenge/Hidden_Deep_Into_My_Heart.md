# Hidden Deep Into my Heart — TryHackMe (Web App Hacking)

**Date:** 2026-02-15  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This TryHackMe web challenge focused on uncovering hidden information inside a seemingly minimal web application.  

The objective was to perform proper reconnaissance and identify misconfigurations or exposed secrets that could lead to unauthorized access to a restricted area and retrieve the flag.

---

## 2) Initial hypothesis

My initial hypothesis was that the attack surface would mainly be the web application running on the exposed HTTP service.

I expected:

- Hidden directories or endpoints
- Possibly an authentication bypass
- A low-to-medium complexity vulnerability

I assumed that some form of login bypass or credential-based attack would eventually be required to retrieve the flag.

---

## 3) Tools used

- Nmap  
- Gobuster  
- Burp Suite  
- Browser DevTools  

---

## 4) Approach (high level)

1. Perform full TCP port scan to identify exposed services.  
2. Enumerate directories on the web service.  
3. Manually inspect the website and its source code.  
4. Review `robots.txt` for hidden paths or information disclosure.  
5. Explore newly discovered directories.  
6. Analyze authentication mechanism.  
7. Test simple credential hypotheses before attempting advanced exploitation.  

---

## 5) Results / Evidence (sanitized)

- Only two open ports were exposed: SSH and a web service.
- Directory enumeration revealed a hidden path referenced in `robots.txt`.
- Inside `robots.txt`, a commented string resembled a password.
- A hidden `/administrator` login panel was discovered.
- Authentication succeeded using a common username combined with the exposed string.
- Final state: authenticated access to the vault and successful flag retrieval.

Flag obtained:

`THM{l0v3_is_in_th3_r0b0ts_txt}`

Impact:

- Sensitive information disclosure via `robots.txt`
- Weak credential exposure
- Improper access control to administrative panel

---

## 6) Recommended remediation

- Never store credentials or sensitive strings inside `robots.txt` (even in comments).
- Enforce strong password policies.
- Avoid credential reuse.
- Implement proper access control for administrative endpoints.
- Remove sensitive information from publicly accessible files.
- Conduct regular security reviews and configuration audits.

---

## 7) Lessons learned

- Reconnaissance is critical and often more important than exploitation.
- Always inspect `robots.txt`, comments, and hidden paths carefully.
- Test simple hypotheses (credential reuse, weak passwords) before attempting complex attacks.
- Avoid overcomplicating challenges.
- Cognitive bias can delay obvious solutions.
- Tools support reasoning — they do not replace it.

---

## 8) Links / Resources

- Room link: TryHackMe – Web App Hacking  
- OWASP Testing Guide (Authentication Testing)  
- Notion detailed write-up (public version)  

---