# Signatures Evasion — TryHackMe - Red Team

**Date:** 2025-12-04  
**Type:** TryHackMe / Lab  
**Scope:** Lab / Authorized only  

---

## 1) Context

Classroom-style Red Team lab on modern, tool-agnostic methods to break AV/EDR signatures.  
Goal: understand how signatures are created, how to localize them precisely, and how to modify code/properties/behavior to evade static and behavioral detections.

---

## 2) Initial hypothesis

1. Understand where signatures come from and how they appear in malicious code.  
2. Apply documented obfuscation methods to break static signatures safely.  
3. Use non-obfuscation-based techniques (hash/entropy/behavior changes) to defeat non-function-oriented signatures.

---

## 3) Tools used

- Hex editor  
- PowerSploit `Find-AVSignature`  
- ThreatCheck / DefenderCheck  
- AMSITrigger  
- Compiler/IDE (for code changes & recompilation)  

---

## 4) Approach (high level)

1. **Identify signature locations**
   - Use iterative split-and-test on binaries (manual & automated) to narrow down suspicious byte ranges.
   - Transition from manual splitting (`head`, `dd`, `split`) to automated tools (Find-AVSignature, ThreatCheck).

2. **Automate signature discovery**
   - Run ThreatCheck to recursively scan byte ranges with Defender/AMSI and report triggering offsets.
   - Use AMSITrigger specifically for PowerShell/AMSI runtime signatures.

3. **Break static code-based signatures**
   - Apply layered obfuscation concepts (Obfuscating Methods / Obfuscating Classes).
   - Split/merge methods and classes, rebuild sensitive strings at runtime, proxy or rename signature-bearing elements.

4. **Modify static properties**
   - Change file hashes via recompilation or bit-flipping for closed/signed binaries.
   - Manage entropy by avoiding overly random identifiers and using more “natural” naming.

5. **Tackle behavioral signatures**
   - Study import-based and runtime behavior detections (IAT inspection, hooked APIs).
   - Implement dynamic loading and custom API resolution to reduce obvious import footprints.

6. **Combine everything in a layered pipeline**
   - First: kill specific, high-confidence signatures.  
   - Then: apply broader structural/flow obfuscation and optional packing in a controlled sequence.

---

## 5) Results / Evidence (sanitized)

- Confirmed that **AV/EDR correlates multiple indicators** (bytes, PE structure, entropy, imports, API chains, runtime behavior) rather than relying on a single string or hash.
- Using **Find-AVSignature / ThreatCheck** made it possible to:
  - Narrow down offending byte ranges inside a binary.
  - Identify which parts of the file triggered Defender/AMSI without blind manual trial-and-error.
- **AMSITrigger** successfully isolated PowerShell/AMSI signatures at line/segment level, showing which script portions were responsible for runtime blocks.
- Demonstrated that:
  - Rebuilding strings at runtime (e.g., concatenation, `StringBuilder`) and splitting/merging methods/classes change the byte pattern enough to remove static matches while preserving behavior.
  - Adjusting properties (hashes via small code changes or bit-flips, entropy via more natural identifiers) can bypass hash- and entropy-based detections.
  - Moving from static imports to **dynamic loading** reduces visibility of sensitive APIs in the IAT, though behavioral hooks still observe high-risk operations.
- Overall impact: shifted from “guessing what AV sees” to **systematically locating and neutralizing specific signatures**, then hardening tools with layered obfuscation.

(No real malicious payloads, production EDR configs, or proprietary signatures are included here.)

---

## 6) Recommended remediation

From a defender / detection-engineering standpoint:

- **Use layered detection instead of single-point signatures**
  - Combine strings/bytes with **PE structure, entropy, imports, and runtime behavior**.
  - Design Yara/Sigma rules that tolerate minor code/structure changes (wildcards, regex, heuristic components).

- **Harden against simple string/layout changes**
  - Avoid relying solely on static strings like `"AmsiScanBuffer"` or `"wdigest.dll"`.
  - Incorporate **API sequences and behavioral chains** (e.g., allocation → write → execute, remote thread creation, AMSI bypass patterns).

- **Monitor dynamic loading and custom resolution**
  - Add detections for:
    - heavy use of `LoadLibrary*` / `GetProcAddress`,
    - manual export table walking,
    - suspicious dynamic resolution of injection-related APIs.
  - Treat “hidden imports” as a signal, not just explicit IAT entries.

- **Check file properties and anomalies**
  - Track unusual hash churn for the same tool family (frequent variants of a known loader).
  - Monitor **entropy spikes** and sections mixing very high entropy with injection or reflective-loading behavior.
  - Watch digital signature anomalies (unusual signers, invalid/stripped signatures on otherwise “legit-looking” binaries).

- **Invest in tooling for reverse-signature analysis**
  - Use internal equivalents of ThreatCheck/AMSITrigger to:
    - validate existing signatures,
    - quickly test how resilient rules are to common obfuscation transformations.
  - Continuously iterate rules based on red team feedback and observed evasion techniques.

- **Defense-in-depth**
  - Assume attackers can break individual signatures; rely on:
    - EDR telemetry,
    - memory scanning,
    - network-level indicators,
    - and threat hunting for suspicious chains of activity.

---

## 7) Lessons learned

- A “signature” can be **bytes, strings, hashes, imports, entropy values, or behavioral patterns**—not just one rule in a database.
- Automated tools (ThreatCheck, AMSITrigger, etc.) make it possible to **pinpoint exactly what AV is reacting to**, instead of guessing.
- Static code signatures are often defeated by:
  - rebuilding strings at runtime,
  - splitting/merging methods and classes,
  - renaming or proxying signature-bearing values and structures.
- File hashes and entropy are **properties**, not destiny; they can be shifted with small, controlled changes.
- Behavioral signatures are the hardest to evade:
  - AV/EDR tracks API usage and injection chains, not just payload bytes.
  - Dynamic loading + custom API resolution helps, but does not remove all behavioral visibility.
- Effective evasion means:
  1. **Find the specific signature**,  
  2. **Break it in a targeted way**,  
  3. **Then layer structural/flow/metadata obfuscation on top**.
- The strategic goal: not perfect invisibility, but making **detection and analysis as expensive, noisy, and slow as possible** for defenders.

> “Don’t just obfuscate randomly — find the signature, break it, then bury what’s left under layers.”

---

## 8) Links / Resources

- Room: TryHackMe — Red Team (Classroom Lab)  
- Tools:
  - PowerSploit `Find-AVSignature`
  - ThreatCheck / DefenderCheck
  - AMSITrigger
- Personal notes:
  - Detailed write-up in Notion (private)