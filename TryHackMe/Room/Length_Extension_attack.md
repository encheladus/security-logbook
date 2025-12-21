# Length Extension Attacks — TryHackMe - Red Team

**Date:** 2025-12-20  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room focuses on **length extension attacks** against cryptographic hash functions.  
The objective is to understand how Merkle–Damgård–based hashes can be abused when used incorrectly for integrity or authentication.

---

## 2) Initial hypothesis

Before starting the lab, I expected to find:

- Hashes used directly as signatures or integrity checks
- Constructions of the form `hash(secret || message)`
- Applications trusting raw hash output as proof of authenticity
- Opportunities to append attacker-controlled data without knowing the secret

---

## 3) Tools used

- Hash extender (length extension tooling)
- Browser developer tools
- Basic scripting (Python)
- Manual request tampering

*(No sensitive scripts, secrets, or credentials included)*

---

## 4) Approach (high level)

- Review how cryptographic hash functions process data internally
- Identify Merkle–Damgård hash usage (MD5, SHA-1, SHA-256)
- Analyze signing / verification logic for raw hash misuse
- Reconstruct valid padding and internal hash state
- Append attacker-controlled data to signed inputs
- Validate success through application behaviour rather than cryptographic errors

---

## 5) Results / Evidence (sanitized)

Successful exploitation of multiple length extension scenarios:

- **Forged valid signatures** for extended messages without knowing the secret
- **Unauthorized file access** by extending signed URL parameters
- **Privilege escalation** by modifying signed authentication cookies
- Server-side verification passed silently due to trusted raw hashes

All validation was based on application responses (e.g. access granted, file served), with no sensitive data disclosed.

---

## 6) Recommended remediation

- Never use raw hashes for authentication or integrity
- Replace `hash(secret || message)` with **HMAC**:
  - HMAC-SHA-256 or stronger
- Avoid deprecated hash functions:
  - Remove MD5 and SHA-1 entirely
- Enforce strict parsing and validation of signed data
- Prefer established standards and libraries over custom cryptographic logic

---

## 7) Lessons learned

- Length extension attacks exploit **design flaws**, not broken algorithms
- SHA-256 is still vulnerable when misused
- Knowing a hash often means knowing the **internal state**
- Cryptographic misuse can lead directly to authorization bypasses
- Proper constructions matter more than algorithm choice

---

## 8) Links / Resources

- TryHackMe — Red Team (Length Extension Attacks)
- Merkle–Damgård construction overview
- OWASP Cryptographic Failures Cheat Sheet
- Public write-up (if published)

---