# Bypass Really Simple Security — TryHackMe - Web App WordPress

**Date:** 2026-01-13  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Study of an authentication bypass affecting a WordPress security plugin (Really Simple Security) identified as **CVE‑2024‑10924**.  
The objective was to understand how logic flaws in REST API authentication flows can fully bypass login and 2FA protections.

---

## 2) Initial hypothesis

- A REST API endpoint related to 2FA onboarding could be abused
- Authentication checks might exist but be incorrectly enforced
- Logic flaws (check vs enforcement) rather than missing validation were likely responsible
- Successful exploitation could result in unauthorized admin access

---

## 3) Tools used

- Burp Suite (Proxy / Repeater)
- Web browser developer tools
- REST API inspection
- TryHackMe provided environment

*(No credentials, payloads, or exploit scripts documented)*

---

## 4) Approach (high level)

- Reviewed the WordPress authentication architecture and REST API exposure
- Identified authentication‑related REST endpoints linked to 2FA onboarding
- Analyzed the logical flow between validation and session establishment
- Traced how user‑controlled parameters propagate through the authentication process
- Observed the system behavior when validation conditions failed

---

## 5) Results / Evidence (sanitized)

- Authentication succeeded despite invalid or missing verification data
- WordPress session cookies were issued without completed authentication
- Access to restricted administrative pages was granted
- Two‑Factor Authentication was bypassed entirely
- Final state observed: authenticated admin session without credentials

---

## 6) Recommended remediation

- Enforce strict validation of authentication function return values
- Immediately halt execution when authentication checks fail
- Prevent session cookie creation unless authentication is explicitly confirmed
- Restrict sensitive REST API endpoints to authenticated contexts
- Add server‑side authorization checks independent of client‑supplied parameters
- Maintain database‑level and application‑level integrity controls
- Keep security plugins updated with automatic patching enabled

---

## 7) Lessons learned

- Authentication logic flaws can be more critical than missing validation
- Secure functions are ineffective if their results are ignored
- REST APIs are high‑risk attack surfaces when exposed unauthenticated
- 2FA can be bypassed through logic gaps even when cryptographically sound
- Security plugins themselves are prime targets and require scrutiny
- Flow control errors can lead to full system compromise

---

## 8) Links / Resources

- TryHackMe room: *Bypass Really Simple Security*
- CVE‑2024‑10924 advisory (Wordfence)
- WordPress REST API documentation
- Really Simple Security official patch notes
