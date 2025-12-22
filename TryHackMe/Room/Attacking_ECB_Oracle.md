# Attacking ECB Oracle — TryHackMe - Red Team

**Date:** 2025-12-21  
**Type:** TryHackMe  
**Scope:** Classroom / Authorized only

---

## 1) Context

This lab focuses on symmetric encryption, specifically AES used in Electronic Codebook (ECB) mode, and demonstrates why ECB is insecure by design.  
The objective is to understand how deterministic encryption leaks structure and enables chosen‑plaintext attacks leading to secret recovery.

---

## 2) Initial hypothesis

Before starting the lab, the expectations were:

- AES is a strong symmetric cipher, but insecure modes can undermine it.
- ECB mode encrypts blocks independently and leaks patterns.
- If attacker‑controlled input is encrypted alongside secret data, ECB may allow plaintext recovery.
- A chosen‑plaintext oracle combined with ECB determinism should enable byte‑by‑byte extraction.
- Proper cryptographic composition (mode + randomness + integrity) is critical for security.

---

## 3) Tools used

- Web request tooling (HTTP client)
- Basic scripting for automation
- HTML parsing utilities
- Cryptography libraries (AES abstraction)

> No credentials, secrets, or sensitive scripts are included.

---

## 4) Approach (high level)

1. Analyze the encryption workflow and identify the cipher mode in use.
2. Observe ciphertext behavior when varying attacker‑controlled input.
3. Infer the block size by measuring ciphertext length changes.
4. Detect ECB usage through repeated ciphertext blocks.
5. Determine alignment between attacker input and secret data.
6. Exploit ECB determinism to isolate unknown bytes.
7. Recover secret data incrementally using ciphertext comparison.

All steps rely on conceptual properties of ECB rather than cryptographic breaks.

---

## 5) Results / Evidence (sanitized)

- Identical plaintext blocks produced identical ciphertext blocks.
- Ciphertext length increased in fixed increments, confirming block encryption.
- Repeated ciphertext blocks revealed alignment and structure.
- Secret data appended to user input was recovered **byte‑by‑byte**.
- No decryption oracle, padding error, or key disclosure was required.

**Final state:** confidential data was fully recovered due to deterministic encryption.

---

## 6) Recommended remediation

- **Never use ECB** for sensitive or structured data.
- Adopt modern authenticated encryption modes:
  - AES‑GCM
  - AES‑CCM
  - AES‑CBC combined with HMAC (when AEAD is unavailable)
- Enforce:
  - Random IVs or nonces per encryption
  - Integrity and authenticity checks (AEAD)
  - Strict separation between user input and secret data
- Avoid returning raw ciphertext for attacker‑controlled input.
- Educate developers on cryptographic composition, not just algorithms.

---

## 7) Lessons learned

- AES is not the weakness; ECB is.
- Deterministic encryption violates semantic security.
- ECB leaks structure, repetition, and alignment.
- Chosen‑plaintext ECB oracles guarantee plaintext recovery.
- Most cryptographic vulnerabilities stem from **architecture and composition**, not math.

---

## 8) Links / Resources

- TryHackMe — Red Team (Classroom)
- NIST Cryptographic Standards (AES & modes)
- OWASP Cryptographic Storage Cheat Sheet
- Public write‑up (Notion / GitHub)

---