# Windows Locale Persistence — TryHackMe / Red Team  
**Date:** 2025-11-25  
**Type:** Classroom  
**Scope:** Lab / Authorized only  

---

## 1) Context

Introductory room on **Windows persistence techniques** in a corporate environment.  
Focus: understanding how attackers maintain access after initial exploitation by abusing accounts, files, services, scheduled tasks, logon mechanisms, and existing applications/databases.

---

## 2) Initial Hypothesis

- Persistence is essential because:
  - Re-exploitation is not always possible.
  - Gaining an initial foothold can be difficult to reproduce.
  - The blue team may evict you at any time.
- Windows offers multiple persistence surfaces:
  - User accounts, privileges, and RIDs.
  - Files and file associations.
  - Services and scheduled tasks.
  - Logon behavior (startup folders, Run keys, Winlogon).
  - Existing services such as IIS and MSSQL.

---

## 3) Tools Used

*(Conceptual only, no payloads or sensitive values)*

- Built-in Windows tools: `net`, `reg`, `wmic`, `sc`, `schtasks`, Task Scheduler, Registry Editor.  
- Administrative tools: PsExec (SYSTEM context), PowerShell.  
- Offensive tooling (lab context):  
  - Payload generation (service-compatible EXEs, reverse shells).  
  - Web shells for IIS (ASP.NET).  
  - MSSQL management tools (e.g., SSMS) for triggers and `xp_cmdshell`.  

---

## 4) Approach (High Level)

1. **Account-based persistence**
   - Studied adding compromised users into powerful local groups (Administrators, Backup Operators, Remote Management Users).  
   - Reviewed direct privilege assignment (e.g., backup/restore privileges) and security descriptors on services/WinRM.  
   - Analyzed **RID hijacking** to make a low-privileged user inherit the Administrator’s token without changing group memberships.

2. **File-based persistence**
   - Examined backdooring of executables to embed payloads while preserving normal functionality.  
   - Explored shortcut (.lnk) hijacking: redirecting shortcuts to scripts that run a payload and then launch the expected program.  
   - Investigated hijacking file associations via the registry so opening a common file type silently executes attacker-controlled code.

3. **Service-based persistence**
   - Created new services configured to run automatically at boot and execute attacker-controlled commands or binaries.  
   - Practiced modifying existing services (path, start type, account) to blend persistence into legitimate service infrastructure.

4. **Scheduled-task persistence**
   - Used `schtasks` to create tasks that periodically execute payloads as SYSTEM.  
   - Hid tasks by manipulating Task Scheduler registry metadata so they continue to run while becoming invisible to normal queries.

5. **Logon-triggered persistence**
   - Leveraged startup folders (per-user and global) to auto-run payloads on interactive logon.  
   - Used `Run` / `RunOnce` registry keys for per-user and system-wide logon execution.  
   - Reviewed Winlogon keys (`Userinit`, `Shell`) as high-reliability, high-impact logon execution points.  
   - Explored per-user logon scripts via `UserInitMprLogonScript`.

6. **Persistence via existing services (IIS & MSSQL)**
   - Deployed web shells into IIS webroots for HTTP-based persistent access using existing application pools.  
   - Implemented MSSQL-based persistence with:
     - `xp_cmdshell` for OS command execution.
     - Database triggers that automatically call remote payloads when specific DB events occur.

---

## 5) Results / Evidence (Sanitized)

- Verified that low-privileged accounts can be turned into powerful persistence anchors by:
  - Adjusting group memberships and privileges.
  - Manipulating RIDs to inherit Administrator tokens.  
- Demonstrated that file-based techniques (executables, shortcuts, file associations) can silently hook into regular user workflows while preserving expected behavior.  
- Confirmed that both newly created and modified existing services can provide reliable, high-privilege persistence across reboots.  
- Showed that scheduled tasks and logon mechanisms can be abused for recurring or logon-based code execution, including hidden or hard-to-detect configurations.  
- Proved that existing infrastructure (IIS, MSSQL) can host long-term persistence in ways that look like normal application/database behavior.  

*(No real usernames, IPs, or credentials recorded.)*

---

## 6) Recommended Remediation

- **Account & privilege hygiene**
  - Limit membership in Administrator/Backup Operators and other powerful groups.  
  - Regularly audit user SIDs/RIDs and detect unusual changes in the SAM.  
  - Monitor assignment of sensitive privileges (backup/restore, impersonation, debug).

- **File integrity & application control**
  - Implement application allowlisting / code signing (e.g., AppLocker) to prevent unauthorized binaries.  
  - Monitor changes to common executables, shortcuts, and file association registry keys.  

- **Service & scheduled-task hardening**
  - Audit services for unexpected binaries, auto-start behavior, or privilege changes.  
  - Periodically review scheduled tasks (including their registry metadata) for unknown entries or missing descriptors.

- **Logon behavior monitoring**
  - Monitor Startup folders and `Run`/`RunOnce` keys (HKCU/HKLM) for new or modified entries.  
  - Alert on suspicious changes to Winlogon values (`Userinit`, `Shell`).  
  - Review per-user environment keys for unusual logon scripts.

- **Web and database defenses**
  - Lock down IIS webroots and restrict who can write to them; use file integrity monitoring.  
  - Harden MSSQL:
    - Disable `xp_cmdshell` where not required.
    - Audit triggers, permissions, and impersonation rules.
    - Monitor for outbound connections initiated by the database engine.

- **Detection & response**
  - Integrate persistence-focused rules in EDR/SIEM (new services, tasks, Run keys, IIS files, MSSQL triggers).  
  - Baseline “normal” persistence locations and alert on deviations.

---

## 7) Lessons Learned

- Persistence is about **maintaining options**, not just running a single payload.  
- You can gain durable, high-privilege access **without** obviously being in the Administrators group (e.g., Backup Operators, RID hijacking, direct privileges).  
- Windows provides many legitimate execution points that can be repurposed:
  - Services, tasks, logon, file associations, web servers, databases.  
- Stealthy persistence often means:
  - Modifying what already exists.
  - Blending into normal automation and maintenance mechanisms.  
- Robust persistence strategies are layered:
  - Quick re-entry (e.g., simple task or service).
  - Long-term, stealthy methods (e.g., IIS web shell, MSSQL trigger, RID hijack).  

---

## 8) Links / Resources

- Room / machine: *TryHackMe — Red Team (Windows Persistence)*  
- Microsoft Docs – Windows services, Task Scheduler, Winlogon, and Run keys  
- IIS documentation (webroot structure, application pools)  
- MSSQL documentation – triggers, `xp_cmdshell`, security best practices  
- Notion / detailed write-up (internal): *add your link here*

---