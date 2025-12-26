# Insecure Randomness — TryHackMe - Web Application Red Team

**Date:** 2025-12-23  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

This lab explores how **incorrectly implemented randomness** leads to full web application compromise.  
The objective is to understand why predictable or weak random value generation breaks authentication, password reset mechanisms, and magic-link logins.

---

## 2) Initial hypothesis

Before starting, I expected vulnerabilities related to:

- Weak or insufficient entropy
- Predictable token generation
- Insecure seeding of PRNGs
- Misuse of non-cryptographic RNGs in security-sensitive contexts

---

## 3) Tools used

- Web browser (manual analysis)
- Burp Suite (proxy & repeater)
- PHP analysis tools
- Entropy / token analysis utilities

*(No credentials, sensitive scripts, or exploit code included.)*

---

## 4) Approach (high level)

1. Analyzed authentication-related workflows (password reset, magic link login).
2. Identified how tokens were generated and what data influenced them.
3. Evaluated randomness sources and entropy quality.
4. Assessed predictability by correlating observed outputs with inputs.
5. Tested whether tokens could be reproduced or guessed within realistic constraints.
6. Validated impact through controlled, authorized interaction with the application.

---

## 5) Results / Evidence (sanitized)

Observed multiple critical weaknesses:

- Password reset tokens derived from **predictable values** (e.g. username + timestamp).
- Authentication tokens generated using **non-cryptographic PRNGs**.
- PRNG seeds influenced by **user-controlled or static data**, making outputs reproducible.
- Small brute-force space allowing rapid token prediction.
- Successful authentication and account takeover without email access.

**Final state:** authentication bypass and full account compromise achieved through token predictability.

---

## 6) Recommended remediation

- Use **CSPRNG-backed APIs** for all security-sensitive randomness.
- Never derive tokens from timestamps, usernames, or predictable inputs.
- Avoid manual seeding of PRNGs.
- Ensure tokens have sufficient length and entropy.
- Implement rate limiting and abuse detection on authentication endpoints.
- Use **authenticated encryption** where encryption is required.

---

## 7) Lessons learned

- Cryptography rarely fails due to broken algorithms — it fails due to **incorrect usage**.
- Randomness is a **security boundary**, not a convenience feature.
- Deterministic behavior in authentication logic is catastrophic.
- Token secrecy must never rely on obscurity or assumed unpredictability.
- Reviewing randomness design often yields **high-impact vulnerabilities** quickly.

---

## 8) Links / Resources

- TryHackMe — Web Application Red Team (Classroom)
- OWASP: Cryptographic Failures
- NIST SP 800-90: Random Number Generation
- Public Notion write-up (sanitized)

---