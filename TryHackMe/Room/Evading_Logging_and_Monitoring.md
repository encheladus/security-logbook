# Evading Logging and Monitoring — TryHackMe - Red Team  

**Date:** 2025-12-07  
**Type:** TryHackMe / Classroom Lab  
**Scope:** Lab / Authorized only  

---

## 1) Context

Red Team classroom lab on TryHackMe focused on bypassing Windows logging and monitoring, especially ETW-based telemetry.  
Objective: understand how ETW works, how PowerShell plugs into it, and how attackers can selectively reduce visibility without breaking the system.

---

## 2) Initial hypothesis

An attacker cannot easily control logs once they leave the host, but can influence:

- What gets logged locally.
- Which providers are enabled.
- How PowerShell and ETW interact in memory.

Hypothesis:

- Targeting **ETW** (Event Tracing for Windows) and **PowerShell logging** at the provider / configuration level allows an attacker to:
  - Silence high-value telemetry (e.g., script block logging),
  - Avoid noisy global ETW tampering,
  - Keep the system “healthy-looking” while hiding their own activity.

---

## 3) Tools used

- **PowerShell**
  - For enumeration of logging configuration and in-memory manipulation.
- **Windows Event Log / Event Viewer**
  - To observe events such as 4103 / 4104 and verify suppression.
- **.NET reflection**
  - To access internal PowerShell and .NET types and private fields.
- **Conceptual C#/Win32 APIs**
  - `LoadLibrary`, `GetProcAddress`, `VirtualProtect`, etc., for understanding ETW function patching.
- **Group Policy / GPO concepts**
  - For understanding how Script Block and Module logging are controlled.

*(No credentials, options or sensitive scripts stored.)*

---

## 4) Approach (high level)

1. **Map the telemetry surface**
   - Review how ETW works (providers, controllers, consumers).
   - Identify PowerShell’s role as an ETW provider and how Script Block / Module logging feed events 4103 and 4104.

2. **Study ETW architecture**
   - Understand the flow: providers → sessions (controllers) → consumers (EDR, SIEM).
   - Identify high-value providers from an attacker’s point of view (PowerShell, CLR, security-related providers).

3. **Analyze PowerShell integration with ETW**
   - Look at the `PSEtwLogProvider` class as PowerShell’s ETW bridge.
   - Understand how flags and internal fields determine whether telemetry gets emitted.

4. **Evaluate evasion strategies at different layers**
   - **Provider level (PowerShell)**
     - Use .NET reflection to locate the internal ETW provider instance and flip the “enabled” flag.
   - **ETW core function**
     - Conceptually review patching `EtwEventWrite` in `ntdll.dll` to force an early return and silence ETW for that process.
   - **Policy / configuration layer**
     - Target PowerShell’s in-memory **Group Policy cache** to disable Script Block and invocation logging without touching real GPO.
   - **Per-module / per-snap-in settings**
     - Use the `LogPipelineExecutionDetails` property to stop pipeline execution logging for specific modules/snap-ins.

5. **Apply in a guided “real world” scenario**
   - Enumerate the environment: check whether Script Block + Module logging are enabled, and whether logs are forwarded off-host.
   - Suppress PowerShell logging in-memory (GPO cache + pipeline flags) in the current session.
   - Clean existing PowerShell logs on the host only if they are not forwarded to a SIEM.
   - Execute the payload and confirm no new 4103/4104 events are generated.

---

## 5) Results / Evidence (sanitized)

Observed behavior in the lab:

- **Before evasion**
  - PowerShell actions produced multiple events:
    - **4104**: Script Block Logging (full script contents, including deobfuscated code).
    - **4103**: Module / pipeline execution details.
  - Any meaningful PowerShell usage risked immediate detection.

- **After reflection-based and policy-based suppression**
  - Modifying the internal **ETW provider flag** for PowerShell made the provider stop emitting ETW events.
  - Adjusting the **in-memory GPO cache** for PowerShell disabled:
    - Script Block Logging (no 4104 events),
    - Script Block invocation logging (no 4103 events).
  - Disabling `LogPipelineExecutionDetails` for key modules/snap-ins removed additional pipeline-related telemetry.

- **Log cleanup (when logs were not offloaded)**
  - Clearing existing PowerShell logs removed traces of earlier enumeration on the local machine.
  - No new entries appeared post-evasion, keeping the operational log effectively empty for PowerShell.

- **Payload execution**
  - With logging correctly suppressed and logs cleared, running the lab payload (`agent.exe`) resulted in success (flag on desktop) and no new 4103/4104 events.
  - Misconfigurations (e.g., forgetting a step or leaving logging enabled) triggered a “Binary leaked, you got caught” style failure, illustrating how residual telemetry exposes the operation.

---

## 6) Recommended remediation

Defensive measures to reduce the effectiveness of these techniques:

1. **Harden PowerShell usage**
   - Enforce constrained language mode where possible.
   - Require signed scripts and restrict interactive PowerShell use for non-admin users.
   - Treat PowerShell as a high-risk interface and limit who can run it.

2. **Protect and monitor logging configuration**
   - Strictly enforce **Script Block Logging** and **Module Logging** via Group Policy.
   - Regularly verify that logging-related registry keys and GPO settings match expected baselines.
   - Alert on unexpected changes to logging policies or disabled providers.

3. **Detect in-memory tampering**
   - Monitor for abnormal modification of:
     - `System.Management.Automation` internals (e.g., cached GPO settings, ETW provider flags).
     - `ntdll.dll` regions, particularly around `EtwEventWrite`.
   - Use EDR rules to detect pages marked **PAGE_EXECUTE_READWRITE** in system DLLs and unexpected patches to common tracing functions.

4. **Telemetry health monitoring**
   - Implement “heartbeats” or sanity checks so the absence of expected ETW/PowerShell events is itself suspicious.
   - Monitor for hosts where:
     - 4103/4104 suddenly disappear,
     - PowerShell logs are repeatedly cleared,
     - Logging volume drops sharply without explanation.

5. **Centralize and protect logs**
   - Forward PowerShell and ETW-derived logs to a central **SIEM**.
   - Treat local log deletion as a low-value obstacle: ensure that once logs are generated, they are preserved off-host.
   - Limit who can clear event logs and alert on these actions.

6. **Application allowlisting and code integrity**
   - Use allowlisting (e.g., AppLocker / WDAC) to restrict unauthorized PowerShell host processes or custom binaries that might perform ETW tampering.
   - Apply code integrity checks on security-sensitive binaries and DLLs.

---

## 7) Lessons learned

- **ETW is a full telemetry pipeline**, not just “some logs”:
  - **Providers** generate events (PowerShell, CLR, kernel, security components).
  - **Controllers** manage sessions and decide what gets recorded.
  - **Consumers** (EDR, SIEM, Event Viewer) interpret and store telemetry.

- **PowerShell is tightly coupled to ETW**:
  - `PSEtwLogProvider`, Script Block Logging, and Module/pipeline logging make PowerShell a very noisy tool by default.
  - Event IDs **4103** and **4104** are particularly dangerous for attackers.

- **Global ETW patching is high-risk and noisy**:
  - Patching `EtwEventWrite` can kill a lot of telemetry at once, but:
    - It is easy to detect.
    - It affects many components.
    - It can destabilize the system or raise integrity alerts.
  - It’s useful to understand as a concept, but not ideal for quiet real-world operations.

- **Fine-grained manipulation is more realistic for Red Team use**:
  - Using **.NET reflection** to:
    - Flip internal PowerShell ETW flags (e.g., `m_enabled`),
    - Modify the **cached Group Policy settings** in memory,
    - Toggle per-module `LogPipelineExecutionDetails`,
    allows per-session stealth without touching disk or GPO.

- **Good OPSEC focuses on prevention, not cleanup**:
  - The goal is to **prevent sensitive logs from ever being generated**, not just delete them afterward.
  - If logs are offloaded to a SIEM, local deletion is almost useless.
  - Order of operations matters:
    1. Enumerate logging and forwarding.
    2. Suppress only the telemetry that matters.
    3. Clean local logs **only if** they’re not offloaded.
    4. Then run the payload.

- **Defenders can use gaps as signals**:
  - A suddenly “quiet” PowerShell provider or missing ETW streams can indicate evasion attempts.
  - Telemetry health monitoring is as important as the telemetry itself.

---

## 8) Links / Resources

- **Room / machine**
  - TryHackMe – Red Team (Logging & ETW Evasion) – *Classroom lab*  
- **Documentation / references**
  - Microsoft Docs – Event Tracing for Windows (ETW) overview  
  - Microsoft Docs – PowerShell Script Block Logging & Module Logging  
  - References on .NET reflection and PowerShell internals (e.g., `System.Management.Automation`)  
- **Notion / detailed write-up**
  - Private Notion page for this lab (source notes for this summary)

---