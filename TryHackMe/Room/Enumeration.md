# Enumeration — TryHackMe / Red Team  
**Date:** 2025-11-24  
**Type:** Classroom  
**Scope:** Lab / Authorized only  

---

## 1) Context

Introductory room on **post-exploitation enumeration** in unknown corporate environments.  
Focus: building a structured, OS-agnostic approach to host and network enumeration on both **Linux** and **Windows** using mostly built-in or commonly installed tools.

---

## 2) Initial Hypothesis

- Enumeration is the bridge between a blind shell and a full understanding of the environment.  
- On **Linux**, it should be possible to map system, users, network, and services using only default tooling.  
- On **Windows**, built-in tools should provide enough visibility into system info, user/group structure, networking, and running services.  

---

## 3) Tools Used

**Linux (typically present by default):**

- System / users: `/etc/*-release`, basic account and group utilities  
- Activity / history: session and sudo-related utilities  
- Networking: IP configuration, routing, and connection inspection tools  
- Processes / services: standard process listing and inspection tools  

**Windows (built-in):**

- System info: OS and patch information utilities  
- Users / groups: identity, privilege, and group membership commands  
- Networking: IP configuration, connection and ARP inspection tools  
- Shares / services: SMB share listing and basic service inventory  

**Conceptual / protocol focus:**

- DNS (including misconfigured zone transfers)  
- SMB file/printer sharing  
- SNMP-based device and system information  

**Additional tooling (awareness only):**

- Sysinternals Suite  
- Process Hacker  
- GhostPack Seatbelt  

*(No options, credentials, or sensitive scripts included.)*

---

## 4) Approach (High Level)

1. **Clarify the purpose of enumeration**  
   Internalized enumeration as a continuous process: turn a single shell into a clear picture of system role, users, network position, and available services.

2. **Linux enumeration (using commonly-installed tools)**  
   - Identified system characteristics and distribution/OS version.  
   - Mapped local users, groups, recent logins, and potential sudo capabilities.  
   - Built an understanding of network configuration, connectivity, and active connections.  
   - Profiled running processes and services to infer the host’s role and potential weaknesses.

3. **Windows enumeration (with built-in tools)**  
   - Collected system information (OS, build, patches, installed applications).  
   - Enumerated users, groups, privileges, and password/account policies.  
   - Assessed network configuration, active connections, and nearby hosts.  
   - Looked for shared resources and internal services that expand attack surface.

4. **Protocol and service-oriented enumeration**  
   - Considered DNS as a discovery source (including the risk of misconfigured zone transfers).  
   - Used SMB concepts to reason about shared folders and potential data exposure.  
   - Treated SNMP as a high-value information leak path where available.

5. **Advanced tooling awareness**  
   - Reviewed how Sysinternals, Process Hacker, and Seatbelt can later refine and accelerate enumeration in real-world engagements.

---

## 5) Results / Evidence (Sanitized)

- Confirmed that **Linux** and **Windows** both expose a large amount of context through default utilities alone (system, users, network, and services).  
- Demonstrated that a structured enumeration sequence avoids random, noisy command use and quickly reveals:  
  - OS version, patch level, and likely vulnerability classes  
  - Privileged accounts, groups, and misaligned permissions  
  - Internal connectivity, potential lateral movement paths, and reachable services  
  - Shared resources and places where credentials or sensitive data might reside  
- Built a reusable mental model for “first steps after a shell” that applies across multiple environments.

*(No real hostnames, usernames, IPs, or configuration details included.)*

---

## 6) Recommended Remediation

- **System hardening**
  - Keep OS and applications patched; remove unused software.  
  - Harden default configurations and restrict unnecessary services.

- **Identity & access**
  - Apply least privilege to users and groups; review administrative memberships regularly.  
  - Enforce strong password and lockout policies at both local and domain levels.

- **Network & services**
  - Limit internal exposure of services; use segmentation and access controls.  
  - Restrict SMB shares to the minimum required; periodically audit permissions.  
  - Secure DNS (disallow unauthorized zone transfers) and SNMP (strong community strings or v3).

- **Detection & monitoring**
  - Log access to sensitive files, configuration changes, and administrative activity.  
  - Monitor for “baseline” enumeration behavior (sudden spikes of system/user/network/service discovery).

---

## 7) Lessons Learned

- **Enumeration is continuous, not a single step.**  
  It starts at first shell and informs every decision afterward (priv-esc, pivoting, data access).

- **Logic is OS-agnostic; commands are not.**  
  On both Linux and Windows, the core questions are the same:  
  *Who am I? Where am I? What is running? What can I reach?*

- **Built-in tools are enough to be dangerous.**  
  Even without uploading custom binaries, you can map system roles, networks, and users with default utilities.

- **Unstructured enumeration = noise.**  
  A checklist-based approach (system → users → network → services → credentials) yields better signal and fewer mistakes.

- **Protocols are intelligence sources.**  
  DNS, SMB, and SNMP can leak huge amounts of information if misconfigured.

- **Advanced tools amplify, not replace fundamentals.**  
  Sysinternals, Process Hacker, and Seatbelt are powerful, but only if the underlying enumeration mindset is already solid.

---

## 8) Links / Resources

- Room / machine: *TryHackMe — Red Team (Enumeration in Corporate Environments)*  
- Microsoft Docs – Windows built-in tools & Sysinternals overview  
- Linux man pages for system, user, network, and process utilities  
- SNMP / SMB / DNS protocol references (for deeper enumeration techniques)  
- Notion / detailed write-up (public or internal): *add your link here*

---