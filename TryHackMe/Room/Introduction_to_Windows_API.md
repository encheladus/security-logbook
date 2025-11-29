# Introduction to Windows API — TryHackMe - Red Team  
**Date:** 2025-11-29  
**Type:** TryHackMe  
**Scope:** Authorized Lab

---

## 1) Context
This lab focuses on the Win32 API: what it is, how it interacts with Windows subsystems and hardware, and how it’s used in real-world offensive tooling and malware.  
Goal: turn the Windows API from “dev documentation” into a practical offensive toolbox.

---

## 2) Initial hypothesis
Before starting, I assumed that:

- The Windows API is the main native interface to interact with core OS components.
- The same API surface is used by:
  - offensive tools / malware
  - EDR / security products
  - legitimate applications
- By understanding:
  1. What the Win32 API is and how it talks to OS subsystems  
  2. How to use it from different languages (C, C#, PowerShell)  
  3. How it shows up in malicious case studies  
  …I would be able to reason about offensive techniques directly in terms of API calls.

---

## 3) Tools used
- Win32 API (KERNEL32, USER32, ADVAPI32, NTDLL)  
- C / C++ with `windows.h`  
- C# / .NET with P/Invoke (`DllImport`)  
- PowerShell with `Add-Type` + P/Invoke  
- Documentation / references:
  - Microsoft Win32 API docs
  - MalAPI / SANS-style API references

_No sensitive options, credentials, or payloads included._

---

## 4) Approach (high level)
1. **Mapped where the API lives in the OS**
   - Revisited user mode vs kernel mode.
   - Positioned Win32 as the “doorway” from userland into kernel services.
   - Connected API calls to system calls and hardware access.

2. **Decomposed the Win32 API into layers**
   - API concept → header/imports → core DLLs → supplemental DLLs → call structures → API calls → parameters.
   - Identified core libraries (`KERNEL32.DLL`, `USER32.DLL`, `ADVAPI32.DLL`, `NTDLL.DLL`) and their roles.

3. **Studied how languages reach the same APIs**
   - C/C++ via `#include <windows.h>` and loader thunk tables.
   - C# (.NET) via `[DllImport("kernel32")]` + correctly mapped signatures.
   - PowerShell via method definitions + `Add-Type` → `.NET` class → static invocations.

4. **Analyzed API call structure and naming**
   - Looked at function prototypes, In/Out parameters, and calling conventions.
   - Understood suffixes (`A`, `W`, `Ex`) and encoding/extended variants.
   - Practiced mapping docs types to language-specific types.

5. **Walked through benign implementations**
   - Simple window creation (`CreateWindowExA`) in C++.
   - Simple info-gathering (`GetComputerNameA`) in C# and PowerShell.

6. **Studied commonly abused APIs**
   - Reviewed high-value calls (`LoadLibraryA`, `GetProcAddress`, `VirtualProtect`, `VirtualAlloc`, etc.).
   - Linked them to typical offensive patterns.

7. **Dissected two practical case studies**
   - C# keylogger using keyboard hooks.
   - C# shellcode launcher using allocate → write → execute chains.

---

## 5) Results / Evidence (sanitized)
- **Clear OS placement:**  
  Confirmed that Win32 is the primary user-mode interface into kernel mode, with tight coupling to process, memory, IO, and GUI subsystems.

- **Structured API mental model:**  
  Built a top-down view:
  - API → Headers/Imports → Core/Supplemental DLLs → Call Structures → API Calls → Parameters.

- **Cross-language understanding:**  
  Saw that:
  - C/C++ uses headers/import libs and the loader.
  - .NET / PowerShell use P/Invoke (`DllImport`, `Add-Type`) to bridge managed and unmanaged code.
  - All ultimately call the same native functions.

- **Common malicious patterns identified:**
  - Recon: `GetUserNameA`, `GetComputerNameA`, `GetVersionExA`, `GetModuleFileNameA`.
  - Execution chain: `VirtualAlloc` / `VirtualAllocEx` → write (e.g., `Marshal.Copy`/`WriteProcessMemory`) → `CreateThread` / `CreateRemoteThread` → `WaitForSingleObject`.
  - DLL abuse: `LoadLibrary` / `LoadLibraryEx` + `GetProcAddress`.

- **Keylogger case (sanitized):**
  - P/Invoke imports from `user32.dll` and `kernel32.dll`.
  - Uses `GetModuleHandle` + `SetWindowsHookEx` with `WH_KEYBOARD_LL` to install a low-level keyboard hook.
  - Maintains a message loop to capture keystrokes.

- **Shellcode launcher case (sanitized):**
  - Uses `VirtualAlloc` to allocate RWX memory.
  - Writes shellcode bytes into the allocated region.
  - Uses `CreateThread` to start execution at the shellcode address.
  - Waits with `WaitForSingleObject` for thread completion.

No sensitive payloads, actual shellcode, or victim-specific data retained—only the structural patterns.

---

## 6) Recommended remediation
- **Monitor and restrict dangerous API patterns**
  - Detect suspicious chains such as `VirtualAlloc`/`VirtualAllocEx` → write → `CreateThread`/`CreateRemoteThread`.
  - Watch for frequent use of `VirtualProtect` changing pages to executable.

- **Harden DLL loading behavior**
  - Safe DLL search mode, signed DLLs, restricted write access to DLL search directories.
  - Flag unusual `LoadLibrary` / `LoadLibraryEx` calls from untrusted directories.

- **Enhance EDR focus on API-level telemetry**
  - Hook high-risk APIs (`SetWindowsHookEx`, `VirtualAllocEx`, `WriteProcessMemory`, `CreateRemoteThread`, etc.).
  - Correlate API usage with process reputation, parent/child relationships, and command-line arguments.

- **Defend against hook-based keylogging**
  - Monitor low-level keyboard hooks (`WH_KEYBOARD_LL`) from non-trusted processes.
  - Restrict or sandbox processes that can install system-wide hooks.

- **Strengthen process isolation and access control**
  - Limit high-privilege handles from low-trust processes.
  - Enforce LSASS and other sensitive process protections.

- **Educate defenders in API-level thinking**
  - Train analysts to read API graphs (alloc → write → exec, hook → capture) rather than only signatures.

---

## 7) Lessons learned
- **Win32 is the central “power layer” of Windows**
  - All serious interaction with OS internals (processes, memory, IO, GUI, registry, services) goes through it.
  - Red-team tools, malware, EDR, and normal apps all share the same API surface.

- **API structure is layered and modular**
  - Core DLLs:  
    - `KERNEL32`: processes, threads, memory, files.  
    - `USER32`: windows, input, hooks.  
    - `ADVAPI32`: registry, services, security.  
    - `NTDLL`: lower-level native APIs and syscall stubs.
  - Supplemental DLLs extend capabilities for domain-specific tasks.

- **Headers vs P/Invoke are just different bridges**
  - C/C++: `windows.h` + thunk tables solve ASLR and address resolution.
  - .NET / PowerShell: P/Invoke (`DllImport`, `Add-Type`) solve the same problem in managed environments.

- **Naming conventions matter**
  - `FunctionA` vs `FunctionW` = ANSI vs Unicode.
  - `FunctionEx` = extended version with more control/options.
  - Picking the wrong variant can cause subtle bugs; picking the right one makes signatures predictable.

- **Malicious behavior is usually a small API graph**
  - Keylogging → `SetWindowsHookEx` (+ `GetModuleHandle` + message loop).  
  - Shellcode → `VirtualAlloc` → write → `CreateThread` → `WaitForSingleObject`.  
  - Recon → `GetUserName`, `GetComputerName`, `GetVersionEx`, etc.  
  Once I see the sequence, I can infer the intent.

- **Managed languages are not “safe by default”**
  - C# and PowerShell can fully abuse Win32 via P/Invoke and `Add-Type`.
  - The same raw primitives are available; they’re just one abstraction layer higher.

- **Deep API knowledge = stronger offense and better defense**
  - As a red teamer, knowing APIs lets me:
    - design my own implants/loaders
    - understand malware samples faster
    - reason about, and potentially evade, EDR logic.

---

## 8) Links / Resources
- TryHackMe — Red Team / Win32 API room  
- Microsoft Win32 API documentation  
- Microsoft Docs on P/Invoke and `DllImport`  
- MalAPI or similar references for commonly abused Win32 calls  
- SANS / community material on Windows API abuse patterns  
- Personal Notion / detailed walkthrough (internal)

---