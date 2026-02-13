# Linux CLI Basics — TryHackMe

**Date:** 2026-02-12  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Introduction to Linux Command Line Interface fundamentals.  
Objective: become comfortable navigating the filesystem, reading files, and performing basic system enumeration in a Linux environment commonly used in cybersecurity.

---

## 2) Initial hypothesis

- Understand what the Linux terminal is and why it is central in cybersecurity.
- Navigate efficiently through the Linux filesystem.
- Identify basic system information after gaining access to a machine.
- Differentiate between kernel information and distribution information.

---

## 3) Tools used

- Linux Terminal (CLI)
- Built-in commands: `pwd`, `ls`, `cd`, `find`, `cat`, `whoami`, `uname`, `df`

---

## 4) Approach (high level)

- Explore filesystem navigation (`pwd`, `ls`, `cd`)
- Identify hidden files and analyze permissions (`ls -l`, `ls -al`)
- Search for files using structured queries (`find`)
- Read file contents (`cat`)
- Perform basic system enumeration:
  - Identify current user
  - Identify kernel and architecture
  - Identify Linux distribution
  - Check disk usage
- Analyze the `/etc` directory for configuration and OS details

---

## 5) Results / Evidence (sanitized)

- Successfully navigated directories using relative and absolute paths.
- Identified current privilege level using `whoami`.
- Retrieved kernel and architecture information using `uname -a`.
- Confirmed Linux distribution via `/etc/os-release`.
- Verified disk usage and mounted filesystems using `df -h`.
- Observed file permissions and understood their impact on access control.

No sensitive data extracted — focus was on methodology and environment understanding.

---

## 6) Recommended remediation

From a defensive perspective:

- Apply strict file permissions (principle of least privilege).
- Regularly audit writable files and directories.
- Restrict access to sensitive configuration files.
- Monitor unusual enumeration patterns.
- Ensure proper separation between user privileges.

---

## 7) Lessons learned

- The terminal is the core interface in cybersecurity operations.
- Enumeration must be structured and intentional.
- Kernel information (`uname`) is not the same as distribution information (`/etc/os-release`).
- Hidden files often contain configuration or sensitive data.
- File permissions directly influence privilege escalation opportunities.
- Mastering basic commands dramatically improves efficiency.
- Filesystem awareness (`/`, `/home`, `/etc`, `/var`, `/tmp`) is fundamental.
- Strong fundamentals compound over time.

---

## 8) Links / Resources

- Room: TryHackMe — Linux CLI
- Personal detailed notes: Notion (public)