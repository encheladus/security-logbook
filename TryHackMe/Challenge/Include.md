# Include — TryHackMe - Web Application Pentesting
**Date:** 2025-11-05  
**Type:** TryHackMe / Challenge  
**Scope:** Lab / Authorized only

---

## 1) Context
Short summary (1–3 lines): Initial web exploitation challenge focused on chaining **server-side** bugs. Goal: pivot from a public app into **internal services** to capture flags and access a hidden admin panel—even when targets aren’t directly reachable from the browser.

## 2) Initial hypothesis
If the app accepts externally controlled URLs (e.g., image/banner fields) and fetches them server-side without strict validation, it’s likely vulnerable to **SSRF**. Internal endpoints might expose secrets/config that can be leveraged to reach an admin panel or escalate further.

## 3) Tools used
Short list:
- Nmap (TCP scan, service discovery)  
- Burp Suite / Caido (HTTP replay & automation)  
- curl (quick header/body checks)  
- ffuf / dirsearch (content discovery)  
- Simple HTTP server (to confirm SSRF callbacks)  
- Browser DevTools (network/console)

*(No options, credentials, or sensitive scripts included.)*

## 4) Approach (high level)
- **Recon**: enumerate open ports/services; fingerprint web stacks; crawl UI.  
- **Privilege hints**: probe UI toggles/IDORs to reveal hidden features (e.g., internal API list).  
- **SSRF check**: supply attacker-hosted URLs in server-fetched fields and observe callbacks.  
- **Pivot**: use SSRF to query **127.0.0.1** internal APIs for secrets/creds.  
- **Lateral step**: authenticate to the internal/admin app with recovered material.  
- **File exposure**: test image/file parameters for **LFI** with traversal bypass variants.  
- **Log poisoning**: inject controlled data into logs, then include them via LFI to coerce code paths that reveal command output.  
- **Evidence**: capture flags and verify impact with minimal, non-destructive PoCs.

## 5) Results / Evidence (sanitized)
What I observed:
- **Hidden UI/IDOR** exposed internal endpoints on `127.0.0.1:5000`.  
- **SSRF confirmed** via image URL fetch (server requested my callback URL).  
- **Internal API** (via SSRF) returned admin credentials (redacted) used to access the second web app (admin panel).  
- **LFI confirmed** on an image parameter; traversal filters blocked `../` but allowed variants such as `....//`.  
- **Log poisoning** worked by inserting attacker-controlled lines in a system log later included via LFI, producing command output as the web user.  
- **Impact**: admin panel access (flag1) and read of web root files (flag2) without direct browser reachability.

*Sanitized proof examples (non-sensitive):*
- “Server fetched `http://attacker/ssrf.txt` (200/404 observed on listener).”  
- “Including `…/var/log/…` via LFI returns log contents; injected marker appears in response.”  
- “Command echo observed in response context as `www-data` (minimal PoC).”

## 6) Recommended remediation
Generic technical measures:
- **Ban open fetchers**: enforce **allow-lists** for server-side URL fetchers; block loopback/metadata/Unix sockets/private RFC1918 ranges; restrict ports.  
- **Harden internal APIs**: require **authN/authZ** even from localhost; avoid embedding secrets in UI/debug tabs.  
- **Prevent LFI**: accept only logical identifiers; map to server paths server-side; **canonicalize** and validate; deny traversal patterns; store media outside the web root.  
- **Sanitize & segregate logs**: treat logs as sensitive input; avoid including raw logs in templates; restrict write/read paths; reduce log injection vectors.  
- **Defense-in-depth**: egress filtering for the web tier, least-privileged FS permissions, AppArmor/SELinux, WAF rules for traversal patterns, secrets management (no hardcoded creds).

## 7) Lessons learned
- **UI leaks + SSRF** are a potent combo: “localhost only” ≠ safe.  
- **Traversal bypass creativity** matters; normalize inputs and validate after canonicalization.  
- **Logs are an attack surface**: if you can write logs and later include them, read-only bugs can become active exploitation paths.  
- Chained bugs (IDOR → SSRF → creds → LFI → log poisoning) require persistence—missing any single link blocks the route.

---

## 8) Links / Resources
- Room / machine link: *TryHackMe — Web Application Pentesting (challenge module)*  
- Useful docs: OWASP SSRF Prevention Cheat Sheet; OWASP Path Traversal; OWASP Logging; PayloadsAllTheThings (SSRF/LFI)  
- Notion / detailed write-up (public): *(add link when published)*

---