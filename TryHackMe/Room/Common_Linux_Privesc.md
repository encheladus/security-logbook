# Common Linux Privesc — TryHackMe

**Date:** 2026-02-02  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

TryHackMe classroom room covering **common Linux privilege escalation techniques**.  
Objective: understand how misconfigurations and weak operational practices enable local privilege escalation to root.

---

## 2) Initial hypothesis

Before starting, I expected the room to focus on:

- Core privilege escalation concepts
- System and user enumeration
- Abuse of SUID / SGID binaries
- Exploiting simple configuration mistakes rather than complex exploits

---

## 3) Tools used

- LinEnum  
- Native Linux utilities  
- GTFOBins (reference)

*(No sensitive scripts, credentials, or exploit code used.)*

---

## 4) Approach (high level)

- Review privilege escalation concepts and attack surface
- Enumerate users, permissions, binaries, cron jobs, and environment variables
- Identify misconfigurations that allow privilege boundary bypass
- Apply known and documented escalation primitives rather than custom exploits

---

## 5) Results / Evidence (sanitized)

Observed multiple **classic privilege escalation vectors**, including:

- Presence of SUID binaries with unsafe execution behavior
- Writable system files owned by privileged users
- Dangerous sudo permissions on interactive binaries
- Scheduled tasks running as root with writable components
- Reliance on `PATH` resolution without absolute paths

**Impact:** ability to escalate from low-privileged user to root through logic flaws and misconfigurations, not memory corruption or zero-days.

---

## 6) Recommended remediation

- Enforce strict file permissions on sensitive system files
- Remove unnecessary SUID / SGID bits
- Use absolute paths in privileged scripts and binaries
- Restrict sudo permissions to non-interactive, minimal commands
- Secure cron jobs and scripts with proper ownership and permissions
- Implement principle of least privilege across users and services

---

## 7) Lessons learned

- Privilege escalation is usually caused by **misconfiguration**, not advanced exploits
- Enumeration quality determines success more than tooling
- SUID binaries are high-risk when combined with environment influence
- Writable `/etc/passwd` or root-owned scripts are effectively game-over
- GTFOBins is a reference manual, not a shortcut
- Slowing down and interpreting system behavior matters more than scanning outputs

---

## 8) Links / Resources

- TryHackMe Room: Common Linux Privilege Escalation  
- GTFOBins documentation  
- Public notes / write-up (if published)

---