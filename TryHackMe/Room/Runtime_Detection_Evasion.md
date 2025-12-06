# Runtime Detection Evasion — TryHackMe - Red Team

**Date:** 2025-12-06  
**Type:** TryHackMe / Lab  
**Scope:** Lab / Authorized only  

---

## 1) Context

Classroom-style Red Team lab on bypassing runtime detections on Windows, focusing on AMSI (Anti-Malware Scan Interface) and how it integrates with PowerShell / .NET runtimes.  
Goal: understand how runtime detections work, how AMSI is wired in, and how attackers bypass or weaken it using tool-agnostic techniques.

---

## 2) Initial hypothesis

- Runtime detections (AMSI, CLR hooks, heuristics) can block payloads even when static AV is evaded.  
- If we understand **where** AMSI and runtime checks sit (CLR / DLR / PowerShell), we can:  
  - Downgrade or avoid them,  
  - Tamper with AMSI internals (reflection, patching),  
  - Or shape payloads so AMSI doesn’t flag them.  
- Expectation: AMSI is very central, but still “just code” running in-process and therefore patchable/abusable.

---

## 3) Tools used

- PowerShell (v5+ and legacy v2)  
- .NET reflection (System.Management.Automation, AmsiUtils)  
- Win32 APIs via P/Invoke (`GetModuleHandle`, `GetProcAddress`, `VirtualProtect`)  
- amsi.fail (online AMSI-bypass snippet generator)  
- AmsiTrigger (AMSI signature trigger analyzer)  

---

## 4) Approach (high level)

1. **Study runtime detections vs classic AV**
   - Compare static AV (disk / hashes / PE) with runtime inspections (CLR/DLR, script engines, JIT).
   - Understand risk scoring: strings, heuristics, behavioral indicators.

2. **Understand AMSI’s role & wiring**
   - Review AMSI as an interface (amsi.dll, `AmsiScanBuffer`, result codes).
   - Map PowerShell integration via `System.Management.Automation.AmsiUtils` and `CompiledScriptBlock.cs`.

3. **Explore AMSI bypass families**
   - **Avoid AMSI**: downgrade to PowerShell v2 (`powershell -Version 2`) where AMSI and modern logging don’t exist.
   - **Lie to AMSI in .NET**: reflection-based bypass (flip `AmsiUtils.amsiInitFailed` to disable scanning in-process).
   - **Patch AMSI code**: locate `AmsiScanBuffer` in `amsi.dll`, change protections with `VirtualProtect`, overwrite first bytes to always return `AMSI_RESULT_NOT_DETECTED`.

4. **Look at automation and signature evasion**
   - Use **amsi.fail** to generate randomized, obfuscated AMSI-bypass snippets (no stable signatures).
   - Use **AmsiTrigger** to identify which script fragments trigger AMSI and then refactor/obfuscate them.

5. **Analyze trade-offs & detection surface**
   - Compare stealth vs reliability of each technique (downgrade vs reflection vs patching vs pure signature evasion).
   - Derive defensive implications and mitigations.

---

## 5) Results / Evidence (sanitized)

- Confirmed that **runtime detections** (AMSI + CLR) can block payloads that already bypass static AV, because:
  - Script content is scanned *as text* in memory before execution.
  - AMSI feeds buffers to Defender or another provider via `AmsiScanBuffer`, returning block/allow codes.
- Observed that:
  - **PowerShell v2 downgrade** removes AMSI and modern telemetry entirely if v2 is still installed/enabled.
  - **Reflection-based AMSI bypass** (`System.Management.Automation.AmsiUtils.amsiInitFailed = true`) disables AMSI for the current process without touching `amsi.dll` code, but is heavily signatured in its naive form.
  - **AmsiScanBuffer patching** via Win32 APIs (GetModuleHandle → GetProcAddress → VirtualProtect → patch opcodes) forces AMSI to always return `AMSI_RESULT_NOT_DETECTED`, effectively blinding it for the session.
- Verified that **automated tools** significantly reduce manual effort:
  - amsi.fail produces fresh, obfuscated AMSI bypasses on demand, each with different structure/signatures.
  - AmsiTrigger pinpoints exactly which strings / code constructs in a script trigger AMSI, enabling precise refactoring/obfuscation rather than blind trial and error.
- No real malware, credentials, or production endpoints were used; all testing is conceptual or lab-based.

---

## 6) Recommended remediation

From a defender / detection-engineering perspective:

- **Eliminate easy bypasses**
  - Remove or disable **PowerShell v2** (optional feature) to block downgrade attacks.
  - Use **AppLocker / WDAC / EDR allowlisting** to prevent launching legacy engines or unapproved interpreters.

- **Harden AMSI and monitor tampering**
  - Monitor for:
    - PowerShell using reflection on `System.Management.Automation.AmsiUtils`.
    - References to `amsi.dll`, `AmsiScanBuffer`, `VirtualProtect`, `GetProcAddress`, `GetModuleHandle` from script contexts.
  - Implement memory integrity checks or EDR rules to detect:
    - Code-section modifications in `amsi.dll`.
    - Known AMSI patch opcode sequences at `AmsiScanBuffer`.

- **Strengthen PowerShell governance**
  - Enforce **Constrained Language Mode** where possible.
  - Enable and monitor script block logging, transcription, and module logging (where compatible with AMSI).
  - Use signed scripts and Execution Policy in combination with application control, not alone.

- **Detect downgrade & bypass patterns**
  - Alert on:
    - `powershell -Version 2` usage.
    - High-risk reflection patterns modifying private/static fields in security-related assemblies.
    - Abnormal use of `Add-Type` importing kernel32 functions from script contexts.

- **Assume attackers have automation**
  - Expect polymorphic AMSI bypass snippets (e.g., via amsi.fail) and evolving obfuscation.
  - Rely less on brittle static signatures, more on:
    - Behavioral detections,
    - Cross-signal correlation (AMSI events + process tree + network + child process behavior),
    - Threat hunting for suspicious patterns rather than exact strings.

---

## 7) Lessons learned

- Runtime detections (AMSI, CLR hooks) are **second-layer defenses** that still see memory-only and dynamically generated payloads.
- AMSI is “just” an interface + DLL:
  - PowerShell integration is implemented in a **very small code path**, but completely controls execution.
  - Fields can be flipped, functions patched, or execution avoided (downgrade).
- Classic one-liner bypasses are widely known and signatured; **structure and semantics matter more than specific strings**.
- Low-level AMSI patching is extremely powerful but also highly visible to modern EDR due to Win32 API and memory behavior.
- The stealthiest long-term approach is often **making scripts look benign to AMSI itself**, using refactoring and automation (AmsiTrigger) rather than obvious runtime tampering.
- Runtime protection is a cat-and-mouse game: static signatures and simple heuristics are fragile, so both attackers and defenders increasingly rely on behavior, context, and automation.

> “AMSI doesn’t read your mind — it reads your strings. Change the strings, change the verdict.”

---

## 8) Links / Resources

- Room: TryHackMe — Red Team (AMSI & Runtime Detection Bypass)  
- Concepts & docs:
  - Microsoft documentation on AMSI (Antimalware Scan Interface)
  - PowerShell / System.Management.Automation internals
- Tools:
  - amsi.fail – automated AMSI-bypass snippet generator
  - AmsiTrigger – AMSI trigger analysis tool for PowerShell scripts
- Personal notes:
  - Detailed write-up in Notion (private)
