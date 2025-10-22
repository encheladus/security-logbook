# Hammer — TryHackMe - Web Application pentesting **Date:** 2025-10-22
**Type:** Challenge
**Scope:** Lab / Authorized only

---

## 1) Context
Use web exploitation skills to bypass authentication on a target website and escalate to remote command execution (RCE).  
Focus areas: weak reset flows, rate limiting, information disclosure, and JWT key handling.

---

## 2) Initial Hypothesis
The challenge likely hinges on weak authentication/session controls. Plan: find an entry (login/reset), pivot via information disclosure to valid identity or token, then escalate privileges to reach an execution surface (direct or indirect) and demonstrate RCE/file read.

---

## 3) Tools Used
- Nmap (service discovery)
- Browser + DevTools (network)
- Burp Suite (interception/inspection)
- ffuf (fast fuzzing / POST fuzz)
- jwt.io (decode/inspect JWTs)

---

## 4) Approach (High Level)
1. **Recon:** Full-port scan to locate nonstandard web service; enumerate paths/files.  
2. **Auth surface review:** Compare login vs reset flows for enumeration leaks and response differences.  
3. **Info disclosure → identity pivot:** Leverage discovered logs/notes to obtain a valid user handle.  
4. **Reset flow testing:** Assess reset code entropy/time window; fingerprint responses to differentiate valid/invalid attempts.  
5. **Throttling analysis:** Map rate-limiting scope (per-IP/per-account); attempt header/source variations (lab context) to understand protection strength.  
6. **Post-login enumeration:** Review dashboard/source and accessible files for secrets, keys, or exec surfaces.  
7. **JWT path validation:** Decode token, inspect headers/claims (`kid`, `alg`, `aud`, `iss`), and test key selection behavior.  
8. **Privilege escalation:** If key material is readable and `kid` is honored, craft a higher-privileged token and target the command/exec endpoint to demonstrate impact (sanitized).

---

## 5) Results / Evidence (Sanitized)
- **Service layout:** SSH on 22/tcp; web app hosted on **1337/tcp** (nonstandard HTTP).  
- **Enumeration:** Public **/hmr_logs** disclosed a valid user: `tester@hammer.thm`.  
- **Reset flow:** **4-digit code** with ~3-minute window; initial throttling was simplistic and tied to source, enabling feasible guessing in a lab setting.  
- **Valid reset → login:** Obtained the correct code, set a new password, and authenticated as the test user.  
- **Post-login artifacts:** Located a **key file** and an **execute_command** feature; page source revealed a **JWT**-protected command UI with restricted verbs.  
- **JWT weakness:** Server honored a user-supplied **`kid`** and loaded a key from a filesystem path readable by the web process. Re-signed a token with elevated `role` and a `kid` pointing to the discovered key path.  
- **Impact:** With the forged token, the command endpoint executed higher-privileged actions, allowing **file read** of a protected flag (conceptual RCE path confirmed).

*(Evidence shown as final states and observations; no secrets, tokens, or sensitive payloads included.)*

---

## 6) Recommended Remediation
- **Reset flow hardening**
  - Increase code entropy (≥6–8 digits or alphanumeric) and shorten validity windows.
  - Enforce **robust** rate limiting & lockouts per account, device, and IP; introduce CAPTCHAs/backoff.
  - Standardize error messages to avoid user enumeration; throttle reset attempts globally.
- **Logging & exposure**
  - Remove public access to logs; redact sensitive data; restrict file listings and debug notes in production.
- **Header trust**
  - Do **not** trust `X-Forwarded-For` from origin traffic; only honor proxy headers at known, terminating proxies that rewrite them.
- **JWT security**
  - Do not accept untrusted **`kid`** values for key lookup; use a **server-side key map**.
  - Keep signing keys **outside** web-accessible/readable paths; least-privilege filesystem ACLs.
  - Enforce allowed algorithms; validate `iss`/`aud`/`exp`; reject unknown `kid`s.
  - Rotate keys and implement strict secret management.
- **Exec surfaces**
  - Remove or strictly isolate any command execution endpoints; add defense-in-depth (allow-listing, non-root service user, sandbox/jail).

---

## 7) Lessons Learned
- **Scan wide first:** Nonstandard ports are common in CTFs; never assume 80/443.  
- **Small leaks → big pivots:** Public logs enabled identity discovery and reset flow targeting.  
- **Low-entropy resets are brittle:** Short PINs + weak throttling are guessable; response fingerprinting is decisive.  
- **Header-based RL can be flimsy:** Proxy header trust must be explicit and controlled.  
- **JWT footguns:** User-controlled key selection (`kid`) + world-readable keys = token forgery and privilege escalation.  
- **“Safe” command UIs aren’t safe:** If auth is bypassed/forged, restricted commands can still yield RCE-class impact.

---

## 8) Links / Resources
- TryHackMe: Web Application pentesting (challenge module)  
- JWT best practices (general)  
- Rate limiting & account lockout guidance (general)

---