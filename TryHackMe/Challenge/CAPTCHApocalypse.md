# CAPTCHApocalypse — TryHackMe

**Date:** 2026-01-05  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Short summary (1–3 lines):

This room focuses on bypassing a weak CAPTCHA implementation protecting an admin login page.  
The objective is to understand how automation, error handling, and flawed authentication logic can be abused despite anti-bot mechanisms.

---

## 2) Initial hypothesis

Before starting, the assumption was that:

- The CAPTCHA could be bypassed or partially automated.
- Error messages might leak information about authentication state.
- A limited password brute-force combined with CAPTCHA handling would be sufficient to reach the admin dashboard.

The attack surface appeared to be a classic login workflow with insufficient defensive depth.

---

## 3) Tools used

- Browser automation framework  
- OCR engine  
- Image preprocessing library  
- Headless Chromium browser  

(Only high-level tools are listed; no options, credentials, or sensitive scripts are included.)

---

## 4) Approach (high level)

The approach followed these conceptual steps:

1. Analyze the login workflow and identify how CAPTCHA and authentication are validated.
2. Observe and classify server-side error messages returned after login attempts.
3. Automate form submission while attempting CAPTCHA recognition.
4. Separate password validation logic from CAPTCHA validation logic.
5. Preserve and retry candidate passwords associated with CAPTCHA-related failures.
6. Iterate until a valid password aligns with a successful CAPTCHA submission.

This method avoids brute-force naïvely and instead exploits logic inconsistencies.

---

## 5) Results / Evidence (sanitized)

Observed outcomes:

- Incorrect passwords consistently returned a clear authentication failure.
- Some attempts returned CAPTCHA-related errors or ambiguous responses.
- These ambiguous responses indicated potentially valid credentials blocked only by CAPTCHA failure.
- By retrying preserved candidates, authentication eventually succeeded.

**Final impact:**  
- Successful admin login  
- Dashboard access  
- Flag retrieval  

No sensitive data, secrets, or internal dumps were required.

---

## 6) Recommended remediation

Generic defensive measures include:

- Normalize authentication error messages to avoid leaking state.
- Implement server-side rate limiting and account lockouts.
- Use transactional or atomic authentication logic.
- Avoid distinguishing CAPTCHA errors from credential errors.
- Enforce strong password policies.
- Treat CAPTCHA as a *supplement*, not a security control.

---

## 7) Lessons learned

- CAPTCHAs do not prevent automation; they only slow it down.
- Error messages are part of the attack surface.
- Even unreliable automation becomes effective with retry logic.
- Authentication defenses must be layered.
- Weak logic is more dangerous than missing protections.

---

## 8) Links / Resources

- TryHackMe Room: CAPTCHApocalypse  
- OCR & CAPTCHA security research  
- OWASP Authentication Cheat Sheet  
- Public notes (if published): Notion / GitHub

---