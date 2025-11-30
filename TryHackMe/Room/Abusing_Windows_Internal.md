# Windows Internals Evasion — TryHackMe Red Team  
**Date:** 2025-11-30  
**Type:** TryHackMe  
**Scope:** Authorized Lab

---

## 1) Context
This lab focuses on leveraging Windows internals components (processes, threads, PE/sections, DLLs, APCs, browser hooks) to evade common detection solutions using tool-agnostic, modern techniques.  
Objective: understand how attackers abuse internal mechanisms to hide, execute, and persist code while staying under EDR / AV radar.

---

## 2) Initial hypothesis
Before starting, I assumed that:

- “Windows internals” covers all core OS components on the back-end (processes, file formats, COM, task scheduling, I/O, etc.).
- These components expose legitimate functionality that can be:
  - abused for stealthy code execution,
  - chained into complex evasion pipelines,
  - combined with real-world malware techniques (e.g., TrickBot).
- By the end, I wanted to:
  1. Understand how internal components are vulnerable.  
  2. Learn how to abuse and exploit Windows internals for evasion.  
  3. Understand mitigations and detections for each technique.  
  4. Map everything to a real-world adversary case study.

---

## 3) Tools used
- Windows API (OpenProcess, VirtualAllocEx, WriteProcessMemory, CreateRemoteThread, etc.)  
- C / C++ for injector and hollowing implementations  
- OS structures: PE headers, sections, thread contexts, APCs  
- Documentation & threat intel:
  - MITRE ATT&CK (T1055 and variants)
  - Public write-ups on TrickBot and browser injection

_No options, payloads, or sensitive code included._

---

## 4) Approach (high level)
1. **Revisit process and memory fundamentals**
   - Broke down process components (VA space, handles, security token, threads).
   - Reviewed virtual memory behavior and PE layout (headers, sections, RVAs).

2. **Study core process injection patterns**
   - Basic shellcode injection (handle → alloc → write → execute).
   - Process hollowing (suspended legit process → hollow → map malicious PE).
   - Thread hijacking (existing thread → suspend → change RIP → resume).
   - DLL injection (Write DLL path → LoadLibrary in remote thread).

3. **Explore alternative execution techniques**
   - Local function pointer execution (no APIs, just direct call).
   - APC-based execution (QueueUserAPC + alertable threads).
   - Section/PE manipulation (overwriting existing sections and entry points).

4. **Map techniques to EDR detection surfaces**
   - Which APIs/behaviors are commonly hooked or flagged.
   - How each method tries to reduce or reshape those indicators.

5. **Analyze a real-world case (TrickBot browser injection)**
   - Process enumeration & targeting browsers.
   - Reflective injection into browser processes.
   - Inline hooking (relative JMP patches) and memory protection changes.
   - In-browser credential / session theft.

6. **Synthesize everything into an evasion mental model**
   - From “individual tricks” to “full evasion pipelines.”
   - Technique selection depending on environment constraints.

---

## 5) Results / Evidence (sanitized)
**Shellcode injection (baseline pattern)**  
- Sequence confirmed:
  - `OpenProcess` → `VirtualAllocEx` → `WriteProcessMemory` → `CreateRemoteThread`.
- Payload runs inside target process context, inheriting its privileges and trust.

**Process hollowing**
- Created a legitimate process (e.g., `svchost.exe`) in `CREATE_SUSPENDED` state.  
- Parsed malicious PE headers (DOS, NT, Optional, sections).  
- Unmapped the original image using `ZwUnmapViewOfSection`.  
- Allocated memory at the preferred image base and wrote headers/sections.  
- Updated thread context (EAX/RIP to new entry point) and resumed execution.  
- Result: target appears as a legitimate process on disk & command line, but executes a malicious PE in memory.

**Thread hijacking**
- Enumerated threads using `CreateToolhelp32Snapshot` + `Thread32First/Next`.  
- Located a thread belonging to the target process and opened it via `OpenThread`.  
- Suspended the thread, retrieved context (`GetThreadContext`), rewrote RIP to shellcode, applied via `SetThreadContext`, and resumed it.  
- Result: code executed inside an existing thread → no new remote thread created, improving stealth.

**DLL injection**
- Located target PID via process enumeration.  
- Allocated memory for DLL path (`VirtualAllocEx`), wrote it into remote memory (`WriteProcessMemory`).  
- Resolved `LoadLibraryA` and used `CreateRemoteThread` to load the DLL inside the process.  
- Result: payload executed as a DLL under target process identity.

**Alternative execution paths**
- **Function pointer execution:**  
  - Cast local RWX memory address to a function pointer and called it directly.  
  - No API calls involved, reducing hooks & behavioral traces (local-only scenario).
- **APC-based execution:**  
  - Used `QueueUserAPC` to schedule shellcode for execution in a target thread once it becomes alertable.  
  - Avoided explicit `CreateRemoteThread`, but relied on thread state and often monitored by EDRs.
- **Section / PE manipulation:**  
  - Conceptually reused existing sections (e.g., `.text`, `.data`) and entry points instead of new allocations.  
  - Reduced suspicious RWX allocations at the cost of higher complexity.

**TrickBot browser injection & hooking (sanitized)**
- Enumerated processes and targeted browsers (`chrome.exe`, `firefox.exe`, `iexplore.exe`, `microsoftedgecp.exe`).  
- Used `OpenProcess` for handle acquisition and reflective injection into browser processes.  
- Modified memory protections using `VirtualProtectEx` to allow code patching.  
- Inserted inline hooks using a `0xE9` relative JMP from the original function to TrickBot’s hook.  
- Restored memory protections afterward to reduce detection.  
- Result: in-browser credential/session theft with high stealth.

All observations are conceptual and sanitized—no real payloads, IoCs, or victim data.

---

## 6) Recommended remediation
- **Monitor classic injection chains**
  - Detect combinations like:
    - `OpenProcess` → `VirtualAllocEx` → `WriteProcessMemory` → `CreateRemoteThread`.
    - `VirtualAlloc` / `VirtualProtect` creating RWX regions in suspicious processes.
  - Correlate with process reputation, ancestry, and command-lines.

- **Harden sensitive processes**
  - Apply protection to LSASS, browsers, and critical services (e.g., PPL, restricted handle access).
  - Reduce privileges where possible and enforce strong sandboxing/isolation.

- **Detect thread hijacking & APC abuse**
  - Monitor frequent use of `SuspendThread` / `SetThreadContext` / `QueueUserAPC` on high-value processes.
  - Flag unusual context changes or APC queues from untrusted origins.

- **Validate in-memory integrity**
  - Compare loaded module code segments against clean baselines to detect inline hooks (patched prologues, unexpected `0xE9` jumps).
  - Use kernel-mode or hypervisor-based integrity checks to bypass userland unhooking.

- **Limit DLL injection vectors**
  - Enforce signed DLL policies where possible.
  - Harden DLL search paths and disable unsafe search orders.
  - Monitor `LoadLibrary*` use from low-trust processes loading DLLs from non-standard locations.

- **Invest in behavior-based EDR**
  - Focus on **behavior graphs** (sequence of operations) rather than only static signatures.
  - Include telemetry around memory protections, thread operations, and unusual in-process code modifications.

---

## 7) Lessons learned
- **Windows internals are the core attack surface for evasion.**  
  Processes, threads, virtual memory, PE structures, sections, and APCs are all legitimate, predictable mechanisms that can be bent toward malicious goals.

- **“Injection” is a family of strategies, not a single trick.**  
  Depending on constraints:
  - If new threads are watched → hijack an existing thread.  
  - If RWX regions are flagged → manipulate existing sections/entry points.  
  - If API calls are heavily hooked → reduce API usage or move closer to syscalls/function pointers.

- **PE literacy unlocks advanced techniques.**  
  Understanding headers, sections, RVAs, and relocations is what makes:
  - process hollowing,
  - manual mapping,
  - and stealth in-memory execution  
  possible.

- **EDRs see behaviors, not “evilness”.**  
  They react to patterns:
  - RWX allocations
  - remote thread creation
  - `WriteProcessMemory` chains
  - inline patches  
  Evasion = designing execution paths that avoid or disguise these patterns.

- **Hooking is a post-injection superpower.**  
  Once inside a process, inline hooks let you:
  - intercept sensitive data (credentials, tokens, sessions),
  - alter control flow,
  - and stay fully in-process (like TrickBot in browsers).

- **Reflective, in-memory, API-minimal techniques are the future of stealth.**  
  The more you:
  - avoid touching disk,
  - reduce suspicious APIs,
  - and live inside trusted processes,  
  the harder you are to detect.

---

## 8) Links / Resources
- TryHackMe — Red Team / Windows Internals Evasion room  
- MITRE ATT&CK T1055 (Process Injection) and sub-techniques  
- Public TrickBot analysis & browser hooking write-ups  
- Microsoft documentation on:
  - Process & thread APIs
  - Virtual memory management
  - PE file format
  - Asynchronous Procedure Calls (APCs)

---