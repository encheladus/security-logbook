# [What the shell?] — TryHackMe - Junior Pentester  
**Date:** 2025-10-10  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context  
Intro to obtaining shells after RCE, choosing **reverse** vs **bind**, and making unstable shells usable (PTY upgrades, `rlwrap`, `socat`, and HTTPS/TLS-wrapped shells).

## 2) Initial hypothesis  
Move reliably from “I can run commands” to a **stable, interactive shell**. Compare reverse vs bind use-cases, practice stabilization, try encrypted shells for stealth, and build a small set of repeatable one-liners.

## 3) Tools used  
ncat/netcat (`nc`), socat, Metasploit (`exploit/multi/handler`), msfvenom, Python/Perl (`pty` tricks), PowerShell, `rlwrap`.

## 4) Approach (high level)  
- Clarify shell types (**reverse** connects back; **bind** listens on target).  
- Practice receiving/connecting and then **stabilizing** shells (PTY, `rlwrap`, `socat`).  
- Add **TLS** to shells (OpenSSL with `socat`) for encrypted transport.  
- Review **staged vs. stageless** payloads and when `multi/handler` is required.  
- Collect cross-OS snippets into a reusable, paste-ready kit.

## 5) Results / Evidence (sanitized)  
- Reverse shell captured from target; initial TTY was fragile (no job control).  
- Upgraded to a **PTY-backed** session (arrow keys/Ctrl+C working).  
- Achieved a **robust TTY** via `socat` with proper signal handling.  
- Confirmed **TLS-wrapped** reverse shell (traffic encrypted on the wire).  
- Verified Meterpreter/staged payloads require **Metasploit handler** (plain `nc` insufficient).

## 6) Recommended remediation  
- Defenders: **block egress** where possible; restrict outbound to required destinations/ports.  
- Enforce **least privilege** and harden services (no `-e` netcat, remove compilers/tools on prod).  
- Monitor for **abnormal listeners** and suspicious outbound connections.  
- Use **application allow-listing**, EDR, and network IDS with TLS inspection where appropriate.  
- Prefer **key-based auth** and segment management networks.

## 7) Lessons learned  
- **Reverse vs bind** is primarily a **firewall/NAT** decision.  
- Immediate **stabilization** (PTY/`socat`) saves time and prevents session loss.  
- **Encrypted shells** hide plaintext but don’t bypass reachability rules.  
- `multi/handler` is **mandatory** for staged/Meterpreter payloads.  
- Netcat variants differ; have **FIFO / PowerShell** fallbacks.

## 8) Links / Resources  
- TryHackMe room: *Junior Pentester — Shells*  
- PentestMonkey reverse shells (php / bash)  
- `socat` docs & static binaries references  
- Metasploit docs: `multi/handler`, msfvenom payloads

---