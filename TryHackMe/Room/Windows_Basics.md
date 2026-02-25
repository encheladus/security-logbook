# Windows Basics — TryHackMe

**Date:** 2026-02-25  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Introduction to the Windows operating system through the TryHackMe Windows room.  
The objective was to understand how to navigate the GUI, manage files, configure the system, and explore built-in security features from both a user and security perspective.

---

## 2) Initial hypothesis

Before starting, I expected the room to focus on:

- Navigating the Windows graphical interface (Desktop, Taskbar, Start Menu)
- Understanding account types and privilege levels
- Managing files and interpreting Windows file paths
- Exploring built-in administrative and security tools
- Connecting basic system usage with cybersecurity concepts such as privilege escalation and attack surface

---

## 3) Tools used

- Windows GUI (Desktop, Start Menu, Settings)
- File Explorer
- Task Manager
- Windows Security
- Windows Defender Firewall
- Windows Update

---

## 4) Approach (high level)

- Explored the Windows interface and core components (Desktop, Taskbar, Start Menu).
- Reviewed authentication mechanisms and different account privilege levels (Guest, Standard, Administrator).
- Navigated the file system using File Explorer and analyzed directory structures.
- Retrieved system information (OS version, device specifications).
- Studied application lifecycle management (installing, updating, uninstalling).
- Examined Windows Update and patch management concepts.
- Analyzed running processes and system resource usage via Task Manager.
- Explored built-in security mechanisms including Windows Security and the host-based firewall.

The focus remained conceptual and practical, without relying on advanced commands.

---

## 5) Results / Evidence (sanitized)

By the end of the lab, I was able to:

- Navigate confidently within the Windows GUI.
- Identify privilege levels and understand their impact on system control.
- Interpret full file paths such as `C:\Users\Administrator\Desktop`.
- Retrieve OS version and system specifications for vulnerability assessment context.
- Monitor running processes and identify PIDs using Task Manager.
- Understand how patch levels affect exploitability.
- Analyze firewall profiles (Domain, Private, Public) and their role in network filtering.

Final state: successful understanding of Windows fundamentals from both operational and security viewpoints.

---

## 6) Recommended remediation

From a defensive standpoint, best practices include:

- Enforcing least privilege (avoid unnecessary Administrator accounts).
- Keeping the operating system and applications updated.
- Regularly reviewing installed software to reduce attack surface.
- Monitoring running processes for abnormal behavior.
- Properly configuring Windows Defender Firewall profiles.
- Leveraging built-in security features (real-time protection, controlled folder access).

---

## 7) Lessons learned

- Understanding how an OS works is essential before attempting to exploit it.
- Privilege levels are central to both defense and offensive security.
- Patch management directly impacts exploit feasibility.
- File paths and directory structures are critical in post-exploitation scenarios.
- Host-based firewalls and endpoint protection are key defensive layers.
- Strong fundamentals reduce confusion during real-world pentesting engagements.

---

## 8) Links / Resources

- Room: TryHackMe – Windows  
- Microsoft Windows official documentation  
- Personal Notion notes (public version)

---