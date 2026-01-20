# Sudo Buffer Overflow — TryHackMe - Sudo Hacking

**Date:** 2026-01-19  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Tutorial-style room on TryHackMe exploring **CVE-2019-18634**, a buffer overflow vulnerability in the Unix `sudo` program.  
Objective: understand how misconfigurations combined with memory corruption can lead to **local privilege escalation to root**.

---

## 2) Initial hypothesis

Before starting, I expected that:
- A misconfigured `sudo` setup combined with an input-handling flaw could allow privilege escalation
- The vulnerability would be reachable even without explicit sudo privileges
- A buffer overflow would likely be detectable via a crash before achieving full exploitation

---

## 3) Tools used

- TryHackMe provided VM  
- Precompiled public exploit (lab-provided)  
- Standard Linux environment tools  

*(No sensitive options, payloads, or credentials used or disclosed.)*

---

## 4) Approach (high level)

1. Reviewed the vulnerability background and affected `sudo` versions  
2. Studied the role of `/etc/sudoers`, specifically the `pwfeedback` option  
3. Analyzed how password input is handled internally by `sudo`  
4. Confirmed the presence of a buffer overflow condition via a crash (PoC)  
5. Studied a reliable public exploit to understand how control is achieved  
6. Executed a precompiled exploit to safely validate privilege escalation  

---

## 5) Results / Evidence (sanitized)

- Providing excessive password input caused `sudo` to crash, confirming memory corruption
- The crash demonstrated a reachable buffer overflow condition
- Executing a known reliable exploit resulted in:
  - Privilege escalation
  - Successful root-level access
- No sensitive data, tokens, or internal memory structures were exposed or recorded

**Final state:** root shell obtained on the vulnerable system

---

## 6) Recommended remediation

- Upgrade `sudo` to a patched version (≥ 1.8.26)
- Disable `pwfeedback` in `/etc/sudoers`
- Enforce strict bounds checking on all user input
- Apply defense-in-depth:
  - Secure defaults
  - Compiler hardening flags
  - Memory protection mechanisms
- Regularly audit privileged binaries and configurations

---

## 7) Lessons learned

- Configuration matters as much as permissions  
- Cosmetic features can introduce critical vulnerabilities  
- A crash only proves *reachability*, not exploit success  
- Privilege escalation can occur **before authorization checks**  
- Studying mature public exploits is a powerful learning accelerator  

---

## 8) Links / Resources

- TryHackMe room: Sudo Buffer Overflow  
- CVE-2019-18634 advisory  
- Public exploit analysis by Saleem Rashid  
- Personal Notion write-up (sanitized, public)

---