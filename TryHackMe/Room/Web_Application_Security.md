# Web Application Security — TryHackMe
**Date:** 2026-02-06  
**Type:** TryHackMe / Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context
A web application is a type of application that can be used directly through a web browser, without the need to download or install anything on the user’s device. Access is provided through a standard, modern browser such as Google Chrome or Safari, combined with an active internet connection.

This is made possible by servers that run continuously. A server is a computer system designed to operate 24/7 in order to *serve* clients. In the context of web applications, the server hosts and runs the application’s code and responds to requests sent by users’ browsers. From the user’s perspective, the application feels local, but in reality the program is executed remotely on the server.

Web applications often rely on multiple backend systems to function correctly, especially databases that store and manage information. For example, an e-commerce website typically uses several databases to handle different types of data, such as customers, products, and sales. These components work together to enable core features like:

- Creating and managing user accounts
- Searching and browsing products
- Purchasing items and tracking orders

---

## 2) Initial hypothesis
Because web applications are publicly accessible and constantly online, they are frequent targets for security attacks. Understanding how they are structured and how data flows between the browser, server, and databases is a key first step toward identifying common security issues and learning how to exploit or secure them in a pentesting context.

---

## 3) Tools used
- Browser (Chrome / Safari)
- URL manipulation for testing object identifiers
- General web application testing knowledge  
**Note:** No sensitive scripts or credentials included.

---

## 4) Approach (high level)
1. Understand the architecture of a web application: browser → server → databases.  
2. Analyze authentication and authorization mechanisms to identify weak points.  
3. Explore common web security issues:
   - Identification & authentication failures
   - Broken access control (including IDOR)
   - Injection vulnerabilities
   - Cryptographic failures
4. Test practical scenarios such as manipulating sequential IDs in URLs to verify access control.

---

## 5) Results / Evidence (sanitized)
- Successfully demonstrated **IDOR vulnerability** by accessing other users’ data through predictable IDs in URLs.  
- Observed insecure password storage (plain text or weak hash) in test scenarios.  
- Some pages did not enforce HTTPS, exposing sensitive data during transmission.

**Impact:**  
- Unauthorized access to sensitive resources  
- Potential account compromise  
- Increased risk of further exploitation due to broken access control

---

## 6) Recommended remediation
1. **Implement strict access control checks** on every request:
   - Validate user permissions for requested resources  
   - Do not rely solely on object ID presence
2. **Use unpredictable identifiers**:
   - Replace sequential IDs with random or hashed values
3. **Secure authentication mechanisms**:
   - Enforce strong password policies
   - Limit login attempts
   - Store passwords securely with modern hashing (bcrypt / Argon2 + salt)
4. **Enforce HTTPS** for all sensitive data transmission
5. **Regular testing and monitoring**:
   - Automated penetration tests and vulnerability scans

---

## 7) Lessons learned
- IDOR occurs due to **missing authorization checks**, not only predictable IDs.  
- Simple issues (like sequential IDs) can have severe consequences.  
- Every request must be validated according to user roles and ownership.  
- Cryptography and authentication are essential, not optional.  
- Manual testing by parameter manipulation is effective in identifying access control flaws.

---

## 8) Links / Resources
- [TryHackMe – Intro to Offensive Security](https://tryhackme.com/room/introtowebappsec)  
- OWASP Top Ten Web Security Risks: [https://owasp.org/www-project-top-ten/](https://owasp.org/www-project-top-ten/)  
- Public write-ups / notes: Notion (internal)