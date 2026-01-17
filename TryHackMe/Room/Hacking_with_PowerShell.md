# Hacking with PowerShell — TryHackMe - PowerShell Hacking

**Date:** 2026-01-17  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

TryHackMe classroom room focused on **PowerShell fundamentals** for security practitioners.  
Objective: understand PowerShell’s object-oriented model, enumeration capabilities, and basic scripting for Windows post-exploitation and reconnaissance.

---

## 2) Initial hypothesis

Before starting, I expected the room to cover:

- What PowerShell is and how it differs from traditional shells  
- Core PowerShell cmdlets and syntax  
- Windows enumeration techniques using native tooling  
- Introductory PowerShell scripting for automation and recon tasks  

---

## 3) Tools used

- PowerShell  
- PowerShell ISE  
- Native Windows cmdlets (Get-*, Select-*, Where-*, Test-NetConnection)  

---

## 4) Approach (high level)

- Studied PowerShell fundamentals (objects, cmdlets, pipeline)
- Practiced command discovery using `Get-Command` and documentation via `Get-Help`
- Performed Windows enumeration:
  - Users and groups
  - Networking information
  - Running processes and scheduled tasks
  - Files, permissions, and patches
- Analyzed object structures using `Get-Member`
- Wrote and reviewed basic PowerShell scripts for:
  - Port enumeration
  - Keyword and pattern searching in files
- Identified common pitfalls and corrected scripting logic

---

## 5) Results / Evidence (sanitized)

- Successfully enumerated:
  - Local users and groups
  - Listening network ports
  - Installed patches
  - Scheduled tasks and running processes
- Confirmed the effectiveness of PowerShell as a **living-off-the-land** tool
- Built and executed basic scripts for:
  - TCP port checking
  - Recursive file content inspection
- Observed how object-based pipelines enable efficient filtering and automation

_Final state: enumeration and scripting objectives completed as expected._

---

## 6) Recommended remediation

From a defensive perspective:

- Restrict PowerShell usage with **Constrained Language Mode** where possible
- Monitor PowerShell activity via:
  - Script block logging
  - Module logging
- Apply least-privilege principles to reduce enumeration surface
- Enforce execution policy controls and signed scripts
- Regularly audit scheduled tasks and startup scripts

---

## 7) Lessons learned

- PowerShell is **fully object-oriented**, not text-based
- The pipeline (`|`) is central to effective PowerShell usage
- `Get-Member` is essential for understanding and manipulating output
- Native Windows tooling can replace external utilities in restricted environments
- Scripts must be reviewed for **logic correctness**, not just syntax
- Enumeration on Windows spans users, networking, files, processes, tasks, and patches

---

## 8) Links / Resources

- TryHackMe Room: Hacking with PowerShell  
- Microsoft PowerShell Documentation  
- Learn PowerShell in Y Minutes: https://learnxinyminutes.com/docs/powershell/  