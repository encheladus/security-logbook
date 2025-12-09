# Living Off the Land (LOLBAS) — TryHackMe - Red Team  
**Date:** 2025-12-08  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context
Short summary (1–3 lines): This room introduces **Living Off the Land (LotL)** tradecraft used by red teams and adversaries — abusing built-in, Microsoft-signed binaries and utilities (LOLBAS + Sysinternals) to perform file transfer, execution, persistence and evasion with minimal on-disk artifacts. Learning objective: recognise common LOLBAS, how they are abused, and defensive signals to detect them.

## 2) Initial hypothesis
What I expected before starting: adversaries will favor built-in, trusted OS tools to reduce detection and bypass application controls. The lab should demonstrate common LOLBAS patterns (file operations, execution proxies, whitelisting bypasses) and a real-world chaining example showing a fileless campaign.

## 3) Tools used
Short list (no options, credentials or sensitive scripts):
- Windows Sysinternals suite (ProcMon, ProcExp, PsExec, ProcDump, TCPView, PsTools, AccessChk, ADExplorer)
- Sysinternals Live (`\\live.sysinternals.com\tools`) (reference)
- LOLBAS catalog (research/reference)
- Windows native utilities abused in the room: `certutil`, `bitsadmin`, `findstr`, `explorer.exe`, `wmic.exe`, `rundll32.exe`, `regsvr32.exe`, `bash.exe` (WSL), `msbuild.exe`
- Supporting tooling for simulation / lab validation (network capture / logging / EDR telemetry)

## 4) Approach (high level)
- Map the lab objectives to MITRE ATT&CK techniques to focus tests (T1105, T1218, T1197, T1027, etc.).  
- Inventory and categorise candidate LOLBAS by capability: file transfer, execution, persistence, DLL loading, UAC / whitelisting bypass.  
- Build safe, non-production experiments to validate behaviors (observe process tree, network flows, file system traces, and EDR/Windows event logs) without deploying real malicious payloads.  
- Simulate chained techniques (download → decode → in-memory execution) to observe telemetry and defensive signals.  
- Document detection opportunities and validate mitigations (AppLocker/WDAC rules, command-line monitoring, process parent checks).

## 5) Results / Evidence (sanitized)
What I observed: impact (non-sensitive proofs)

- File operations: built-in utilities can be repurposed for ingress/egress and staging.  
  - **Final state (sanitised):** remote artifact successfully staged using a trusted binary and decoded in-memory; no persistent unsigned executable left on disk.  
- Execution proxies: signed system binaries (explorer, wmic, rundll32) can be used to spawn or host payloads in a way that reduces obvious PowerShell/cmd.exe telemetry.  
  - **Final state (sanitised):** execution chain showed a benign-looking parent (e.g., `explorer.exe → child`) rather than an interpreter chain, lowering anomaly score in simple name-based detection.  
- Whitelisting bypasses: regsvr32 / MSBuild / bash (WSL) allow indirect execution that defeats naive allow-listing by name.  
  - **Final state (sanitised):** an execution simulated via a trusted proxy completed without spawning the normally-monitored interpreter process.  
- Real-world validation: Astaroth-style chains (WMIC → BITS → ADS → certutil → regsvr32) illustrate practical, fileless persistence and exfiltration patterns used in the wild.  
  - **Sanitised evidence:** observed layered staging and decoding steps that avoided persistent unsigned binaries; defensive artifacts were limited to network requests and subtle process-parent relationships rather than large on-disk footprints.

> Note: evidence above is described at a high level and intentionally sanitised to avoid sharing sensitive commands or payloads.

## 6) Recommended remediation
Generic technical measures to mitigate these classes of abuse:
- Enforce least privilege: remove admin rights where not required; restrict interactive admin usage.  
- Application control hardening: use AppLocker/WDAC with rules that consider **command-line arguments, child process relationships and user context**, not only binary names.  
- Monitor and alert on execution of high-risk signed binaries (PsExec, ProcDump, RegSvcr32, Rundll32, CertUtil, BitsAdmin, MSBuild, WMIC, Bash) — include contextual correlation (who launched it, from where, parent image, working directory).  
- Network controls: block or proxy unnecessary outbound destinations; monitor for suspicious HTTP(s) requests from system tools.  
- EDR / telemetry: enable detailed process creation logging, process tree capture, module loads and memory dump triggers for suspicious sequences.  
- Sysmon/Windows event logging: deploy Sysmon with rules for command-line, parent-child correlation, “live.sysinternals.com” access, and suspicious use of certutil/bitsadmin/regsrv32.  
- User awareness & mail controls: harden email gateways and sandboxing to reduce LNK / malspam initial access vectors.  
- Audit and block access to `\\live.sysinternals.com\tools` if not operationally required, or monitor it closely.  
- Defensive testing: run purple-team exercises focusing on LOLBAS abuse chains and validate detection coverage end-to-end.

## 7) Lessons learned
- Living Off the Land is fundamentally about **abusing legitimate OS features** rather than relying on dropped malware; defenders must treat legitimate tools as potential attack surfaces.  
- LOLBAS entries are powerful because they are Microsoft-signed and commonly allowed by default; name-based blocking is insufficient.  
- Many administrative binaries possess hidden capabilities (remote downloads, DLL loading, script execution) that can be chained into fileless pipelines.  
- Attackers avoid PowerShell telemetry by executing equivalent functionality through other trusted binaries (MSBuild, rundll32, regsvr32, etc.).  
- Effective detection requires contextual signals: command-line, parent/child relationships, user identity, and unusual hosting locations — not just binary hashes or filenames.  
- Real-world malware (e.g., Astaroth) operationalises these techniques at scale; the threats are practical, not theoretical.

---

## 8) Links / Resources
- TryHackMe — Red Team (Room): *TryHackMe - Red Team room*.  
- LOLBAS project: curated list of living-off-the-land binaries (official LOLBAS repository / website).  
- Sysinternals Suite & Sysinternals Live: reference documentation on tools and `\\live.sysinternals.com\tools`.  
- Recommended readings: MITRE ATT&CK techniques referenced (T1105, T1218, T1197, T1027, etc.), defensive guidance on AppLocker/WDAC and Sysmon.  
- Notion / detailed write-up (public): *[Add your public write-up link here]*

---