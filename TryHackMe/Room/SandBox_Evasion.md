# Red Team — TryHackMe  
**Date:** 2025-12-11  
**Type:** TryHackMe / Classroom  
**Scope:** Lab / Authorized only  

---

## 1) Context

Learn about active defense mechanisms Blue Teams deploy to identify adversaries.  
This room focuses on “Defense in Depth,” malware sandboxes, static/dynamic analysis, and how adversaries evade automated analysis environments.

---

## 2) Initial hypothesis

Before starting the lab, I expected to learn the underlying logic of malware sandboxes and how evasion works.  
Learning objectives included:

- Understanding how sandboxes function  
- Static/dynamic malware-analysis fundamentals  
- Common sandbox-evasion techniques  
- Developing/testing evasion in Any.Run

---

## 3) Tools used

- Any.Run  
- VirusTotal (dynamic)  
- C++ dropper (custom)  
- Python HTTP server  
- MSFvenom  
- Windows APIs (environment checks)  
- Analysis utilities (IDA/Ghidra references, ProcMon, etc.)

*(No sensitive configs or commands included.)*

---

## 4) Approach (high level)

1. Reviewed how sandboxes perform static and dynamic analysis.  
2. Analyzed typical evasion methods: timing delays, CPU-heavy stalling, geolocation checks, hardware/environment fingerprinting, and AD enumeration.  
3. Implemented these techniques in a custom C++ dropper retrieving raw shellcode from an HTTP server.  
4. Executed samples in Any.Run and VirusTotal to compare behaviors and observe detection gaps.  
5. Correlated evasion impact with sandbox limitations (short execution windows, unrealistic hardware, no AD).  
6. Evaluated how to strengthen sandbox realism and detection logic.

---

## 5) Results / Evidence (sanitized)

- Timing-based delays prevented sandboxes from capturing network callbacks or payload execution.  
- Low-resource VMs caused system-profile checks (RAM/CPU/BIOS/MAC) to terminate early.  
- Geolocation checks detected cloud-hosted analysis environments and suppressed execution.  
- Absence of AD membership triggered early exits.  

Outcome: multiple sandboxes labeled the sample *benign/inconclusive*, while on a realistic host the dropper successfully fetched and executed shellcode.

---

## 6) Recommended remediation

- Increase sandbox fidelity: realistic RAM/CPU, disk size, GPU acceleration, non-virtual BIOS strings.  
- Add domain membership to analysis VMs or simulate AD.  
- Extend dynamic-analysis windows or instrument timing primitives (e.g., RDTSC, QueryPerformanceCounter).  
- Detect excessive probing (AD queries, BIOS/MAC checks, public IP lookups).  
- Flag CPU-bound stalling and environmental profiling early in execution.  
- Combine weak behavioral signals (timing + system checks + net probes) into composite detection rules.

---

## 7) Lessons learned

- Sandbox evasion is multi-layered: timing, CPU stalls, system checks, and network/geolocation logic.  
- Short execution windows and unrealistic VM setups make sandboxes easy to evade.  
- Small signals such as early environment probes are strong indicators of malicious intent.  
- Realistic system context (AD, hardware, network traffic) dramatically improves sandbox output.  
- Behavioral correlation is more reliable than relying on a single detection indicator.

---

## 8) Links / Resources

- TryHackMe Room: *Red Team – Sandboxes & Evasion*  
- Malware analysis docs (MSDN, WinAPI references)  
- Public write-up / notes (Notion)  
- Any.Run / VirusTotal sandbox reports

---