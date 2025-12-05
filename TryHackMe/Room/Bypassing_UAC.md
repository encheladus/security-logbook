# Bypass UAC — TryHackMe - Red Team

**Date:** 2025-12-05  
**Type:** TryHackMe / Lab  
**Scope:** Lab / Authorized only  

---

## 1) Context

Classroom-style Red Team lab on common User Account Control (UAC) bypass techniques on Windows.  
Objective: understand UAC internals, integrity levels, and how attackers abuse auto-elevating binaries, scheduled tasks, and automation frameworks to upgrade a Medium Integrity admin shell to High Integrity.

---

## 2) Initial hypothesis

From an attacker’s point of view, UAC is a major obstacle between “local admin membership” and **real** elevated control. I expected to:

- Review how UAC, tokens, and integrity levels actually work.
- Learn multiple UAC bypass techniques (auto-elevate abuse, registry/ProgID tricks, env var expansion).
- See how defenders (e.g., Windows Defender) monitor and react to classic bypasses.

---

## 3) Tools used

- Built-in Windows tools (Task Scheduler, registry editor concepts, environment variables)
- Sysinternals `sigcheck` (manifest inspection / signatures)
- UACME / Akagi (UAC bypass automation)
- Generic reverse shell tooling (e.g., socat/netcat, conceptually)

---

## 4) Approach (high level)

1. **Understand UAC and tokens**
   - Reviewed UAC’s role inside Mandatory Integrity Control (MIC).
   - Differentiated filtered vs elevated tokens and Low/Medium/High/System integrity levels.
   - Looked at UAC settings (Always notify vs default vs disabled).

2. **Study auto-elevating processes**
   - Identified Microsoft-signed binaries in trusted paths that auto-elevate (e.g., `fodhelper.exe`).
   - Inspected manifests and the concept of internal allowlists.

3. **Analyze registry-based UAC bypasses**
   - Explored how ProgIDs and `HKEY_CLASSES_ROOT` (HKCR = HKCU + HKLM) work.
   - Reviewed classic `fodhelper.exe` + `ms-settings` hijack.
   - Improved it using the **CurVer** redirection trick to evade Defender-monitored keys.

4. **Investigate scheduled-task & environment-based bypass**
   - Looked at the **DiskCleanup / SilentCleanup** task configured to “Run with highest privileges”.
   - Abused environment variable expansion (`%windir%`) via user-scoped registry keys to launch a High IL payload, including under *Always notify*.

5. **Automate with UACME**
   - Mapped manual techniques to UACME (Akagi) method IDs.
   - Used Akagi to quickly test different UAC bypass methods and spawn elevated shells.

---

## 5) Results / Evidence (sanitized)

- Confirmed that an account in the **Administrators** group still runs shells with a **Medium Integrity filtered token** by default, blocking many admin operations.
- Observed that **classic fodhelper hijacks** using `HKCU\Software\Classes\ms-settings\Shell\Open\command` are aggressively monitored and remediated by Windows Defender.
- Validated that:
  - **CurVer-based fodhelper bypass** (custom ProgID + `ms-settings\CurVer`) successfully yields a High IL process while avoiding Defender’s monitored key.
  - **DiskCleanup / SilentCleanup** scheduled task can be abused via `%windir%` environment variable override in `HKCU\Environment` to obtain a High IL session even under *Always notify*.
- Verified that UACME (Akagi) method IDs:
  - Automate the classic fodhelper,
  - The DiskCleanup env var trick,
  - And the improved CurVer-based fodhelper bypass,
  providing quick, repeatable elevation tests.

(No real production endpoints or sensitive configurations are included in this write-up.)

---

## 6) Recommended remediation

From a defensive perspective:

- **Harden UAC usage**
  - Prefer **Always notify** on high-value endpoints.
  - Educate that local admin ≠ automatically full control; monitor for unexpected UAC bypass attempts.

- **Reduce auto-elevation attack surface**
  - Inventory and restrict usage of **auto-elevate binaries** where possible.
  - Apply application control (AppLocker / WDAC) to limit which processes can be launched and from where.

- **Monitor and protect critical registry paths**
  - Watch for suspicious writes under:
    - `HKCU\Software\Classes\ms-settings\…`
    - Custom ProgIDs + `Shell\Open\command`
    - `HKCU\Environment` (especially `windir` overrides).
  - Alert on unexpected changes to `CurVer` keys and user-level ProgID overrides.

- **Scheduled task hardening**
  - Audit scheduled tasks that:
    - Run as “Users” but **Run with highest privileges**,
    - Invoke commands via environment variables.
  - Restrict who can trigger sensitive tasks and log task invocations.

- **Behavioral detection**
  - Look for:
    - High IL processes spawning from unusual parents (e.g., `fodhelper.exe` launching shells).
    - Repeated registry modifications followed by launching auto-elevate binaries.
    - Suspicious use of environment variable overrides to change execution paths.

- **Test and validate with red-team tooling**
  - Regularly run frameworks like UACME in a controlled environment to:
    - See which methods still work in your current configuration.
    - Validate that EDR/UAC policies catch or block known techniques.

---

## 7) Lessons learned

- UAC is a **user convenience mechanism**, not a strict security boundary—many bypasses are “by design” and remain unpatched.
- Having **local admin rights** is not enough; what really matters is the **Integrity Level** of the current token.
- Auto-elevating, Microsoft-signed binaries in trusted paths (`fodhelper.exe`, specific MMC snap-ins, etc.) are prime targets for abuse.
- The registry merge view (HKCR from HKCU + HKLM) and **ProgID/CurVer** logic are powerful primitives for hijacking trusted execution flows.
- Defender goes beyond static file scanning: it monitors **specific keys and PowerShell patterns**, forcing attackers to adapt (e.g., `cmd` variants, alternate keys).
- Scheduled tasks with **Run with highest privileges** and environment variable expansion can silently grant High IL execution.
- Automation frameworks like **UACME** are invaluable both for attackers (weaponization) and defenders (testing coverage).

> “UAC bypass isn’t magic — it’s just convincing a trusted High-IL process to run your stuff instead of Microsoft’s.”

---

## 8) Links / Resources

- Room: TryHackMe — Red Team (Classroom Lab on UAC bypass)  
- Official documentation & references:
  - Microsoft docs on User Account Control (UAC) & MIC
  - Application manifests & `autoElevate` settings
- Tools:
  - UACME / Akagi (collection of UAC bypass methods)
  - Sysinternals Suite (e.g., `sigcheck`)
- Personal notes:
  - Detailed write-up in Notion (private)