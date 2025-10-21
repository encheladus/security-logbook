# Multi-Factor Authentication (MFA) — TryHackMe - Web Application Pentesting **Date:** 2025-10-21
**Type:** Classroom
**Scope:** Lab / Authorized only

---

## 1) Context
Exploiting Multi-Factor Authentication.  
MFA adds extra layers beyond passwords to block account takeover. This lab covers factor types, common weaknesses (OTP brute force, token leakage, push fatigue), and beginner-friendly hardening.

---

## 2) Initial Hypothesis
Understand what MFA/2FA adds over passwords, why some methods (e.g., SMS) are weaker, build a mental model of common failures, and learn baseline defenses for a real app.

---

## 3) Tools Used
- TryHackMe Classroom environment
- Browser DevTools (Network tab)
- Burp Suite (interception/inspection)
- Authenticator app (TOTP: Google Authenticator / Authy)
- Test hardware key (e.g., FIDO2/WebAuthn), where available

---

## 4) Approach (High Level)
1. Reviewed MFA factor categories and common 2FA methods (TOTP, push, SMS, hardware keys).
2. Enrolled a test account in a TOTP app; observed ~30s code rotation and clock dependency.
3. Inspected login/MFA traffic to spot OTP handling, tokens in responses, and session transitions (pre-2FA → post-2FA).
4. Purposefully triggered errors to observe rate limiting/lockouts and backoff behavior.
5. Compared channels (SMS vs TOTP vs hardware keys) and explored “push fatigue” and number matching.

---

## 5) Results / Evidence (Sanitized)
- Verified TOTP is **time-based**; incorrect device clock caused valid-looking codes to fail.
- Observed login → MFA flow with a distinct **pre-2FA session**; altering OTP payloads without proper validation was rejected.
- Found no OTPs echoed in API responses (good); noted why returning OTPs/tokens would leak secrets.
- Rate limiting engaged after multiple wrong OTP attempts (progressive delays visible).
- Push prompts required **number matching**, reducing accidental “Approve” clicks.

---

## 6) Recommended Remediation
- **Prefer phishing-resistant factors:** FIDO2/WebAuthn hardware or platform authenticators for high-value accounts.
- **Harden TOTP:** enforce accurate time sync; short validity windows; server-side drift tolerance kept minimal.
- **Strict rate limiting & lockouts:** per account and per IP on OTP entry; add backoff/CAPTCHA on anomalies.
- **Secure token handling:** never return OTPs or verification tokens in client responses; sanitize logs; use `HttpOnly`, `Secure`, `SameSite` cookies.
- **Defend push flow:** require **number matching** and device unlock/biometric; alert on repeated unsolicited prompts.
- **Backup codes:** generate once, show once; encourage storage in a password manager; allow easy rotation.
- **Conditional access:** step-up auth on risky context (new geo/device/time), and re-auth for sensitive actions.
- **Monitoring & alerts:** detect OTP brute patterns, repeated pre-2FA sessions, and unusual device enrollments.

---

## 7) Lessons Learned
- 2FA ⊂ MFA: 2FA = exactly two factors; MFA can be two or more.
- Use **different categories** (e.g., password + authenticator app), not two “things you know.”
- **TOTP depends on time**; clock drift breaks codes.
- **SMS is weakest** (SIM swap/interception); prefer **TOTP** or **hardware keys**.
- **Rate limiting/lockouts** are essential because OTPs are short and guessable.
- **Push fatigue** exists; number matching and user education reduce accidental approvals.
- **Backup codes = secrets**; protect them like keys.

---

## 8) Links / Resources
- TryHackMe: Web Application Pentesting (MFA module)
- FIDO2/WebAuthn overviews (hardware-backed MFA)
- NIST/SP and general guidance on MFA best practices
- Vendor docs: Authenticator apps (TOTP), push providers (Duo/Google Prompt)

---