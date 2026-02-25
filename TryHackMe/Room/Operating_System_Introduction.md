# Operating System Introduction — TryHackMe (Computer Science)

**Date:** 2026-02-24  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Introduction to operating systems through the TryHackMe Computer Science room.  
Objective: understand what an OS is, how it works internally, and why it is fundamental for system security and pentesting.

---

## 2) Initial hypothesis

Before starting, I expected to:

- Understand what an operating system really is (beyond “Windows or Linux”).
- Identify its core responsibilities (processes, memory, files, users).
- Learn the difference between kernel space and user space.
- Recognize how different OS types fit different environments (desktop, server, mobile, embedded).

I also assumed that understanding OS internals would be essential later for privilege escalation and post-exploitation.

---

## 3) Tools used

- Linux CLI (basic system commands)
- Local terminal environment

No exploitation tools were used in this room since it focused on theory and fundamentals.

---

## 4) Approach (high level)

- Studied the layered architecture of a computer system (Hardware → OS → Applications → User).
- Analyzed the OS as an abstraction layer managing hardware access.
- Broke down core OS responsibilities:
  - Process management
  - Memory management
  - File system management
  - User management
  - Device management
- Explored privilege separation (kernel space vs user space).
- Compared GUI vs CLI interaction models.
- Reviewed different OS categories (desktop, server, mobile, embedded, cloud).

The airport analogy was used to conceptualize the OS as a control tower coordinating all system operations.

---

## 5) Results / Evidence (sanitized)

Key understanding achieved:

- The OS is the central authority controlling access to CPU, memory, storage, and devices.
- Applications never directly interact with hardware; they rely on system calls.
- Privilege separation (kernel vs user space) is a core security boundary.
- CLI provides deeper control and efficiency compared to GUI, especially in cybersecurity contexts.
- Different operating systems are optimized for specific environments (e.g., servers prioritize uptime and stability, embedded systems prioritize lightweight performance).

Basic system information gathering was practiced using standard Linux commands (non-sensitive).

---

## 6) Recommended remediation

From a defensive and system design perspective:

- Enforce strict separation between kernel and user space.
- Apply least privilege principles to users and processes.
- Protect critical system files and configurations.
- Ensure proper memory isolation between processes.
- Keep the OS updated to reduce kernel-level vulnerabilities.

Strong OS hardening is foundational before adding additional security layers.

---

## 7) Lessons learned

- The operating system is not just a background program but the core authority of the machine.
- Kernel space is highly privileged and any vulnerability there can lead to full system compromise.
- User space isolation is one of the main reasons modern systems are stable.
- Understanding process and memory management is critical for future privilege escalation techniques.
- Mastering the CLI is essential for cybersecurity and pentesting.

---

## 8) Links / Resources

- TryHackMe – Computer Science Room (Operating System Introduction)
- Linux manual pages (`man`)
- Personal Notion notes (public version)

---
