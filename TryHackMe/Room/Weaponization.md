# Weaponization — TryHackMe / Red Team  
**Date:** 2025-11-20  
**Type:** TryHackMe / Classroom  
**Scope:** Lab / Authorized only  

---

## 1) Context
Short summary (1–3 lines):  
Theory room on **weaponization for initial access**, focused on using native Windows scripting and Office features (WSH, HTA, VBA, PowerShell) to execute payloads, plus a high-level view of C2 frameworks and common delivery methods (email, web, USB).

---

## 2) Initial hypothesis
What I expected before starting:  
- Understand **when and why** to use WSH, HTA, VBA, and PowerShell in real red team ops.  
- Learn how these scripting engines are used to **execute, download, and stage payloads** without dropping obvious `.exe` files.  
- Connect weaponization techniques to **C2 usage** and practical delivery (phishing, web hosting, USB, etc.).

---

## 3) Tools used
Short list — **do not** include options, credentials, or sensitive scripts.  
- Windows Script Host: `wscript.exe`, `cscript.exe` (VBScript).  
- HTA / `mshta.exe` (HTML Applications, hta-psh payloads).  
- Microsoft Office (Word VBA macros, `.docm` documents).  
- PowerShell (in-memory execution, `WebClient`, reverse shells).  
- Payload tooling (PowerCat, `msfvenom`), generic C2 frameworks (conceptual: Cobalt Strike, Empire, Metasploit).  

---

## 4) Approach (high level)
Conceptual steps followed (no precise commands):  
- Reviewed **Windows Script Host (WSH)** to see how `.vbs` (and even `.txt` with `/e:VBScript`) can run commands and spawn binaries.  
- Explored **HTA + mshta.exe** as a LOLBIN-based execution vector, from simple `cmd.exe` spawns to HTA-delivered reverse shells (manual hosting & Metasploit’s `hta_server`).  
- Built basic **VBA macros** in Word, then added auto-execution triggers (`Document_Open`, `AutoOpen`) and looked at macro-enabled formats (`.docm`).  
- Studied **PowerShell execution policy** and common bypasses, then used it as a download-and-execute engine (PowerCat in-memory reverse shell example).  
- Mapped **C2 frameworks** (Cobalt Strike, Empire, Metasploit) to the payloads generated and how they fit into post-exploitation.  
- Reviewed **delivery techniques** (email, web, USB) and how payloads are wrapped to look legitimate and blend into user workflows.  

---

## 5) Results / Evidence (sanitized)
What I observed (impact, non-sensitive):  
- Each technology (WSH, HTA, VBA, PowerShell) provides a different **native path to code execution** without relying on standalone `.exe` files.  
- **WSH** easily runs local binaries and can bypass extension-based controls with `/e:VBScript`.  
- **HTA files** executed via `mshta.exe` can run arbitrary commands and staged payloads from **remote URLs**, making them ideal for initial access and staging.  
- **VBA macros** give a reliable “user opens doc → code runs” pattern via `AutoOpen` / `Document_Open`, especially in macro-enabled formats.  
- **PowerShell** combines powerful OS access with trivial execution-policy bypass and in-memory loading (e.g., `IEX` + `WebClient`), supporting stealthy reverse shells.  
- The main difficulty was **not technical syntax**, but **choosing the right technique** for the environment and seeing how all of them are just different wrappers around the same primitives.  

---

## 6) Recommended remediation
Generic technical measures (defensive side):  
- **Constrain scripting engines**:  
  - Restrict or monitor `wscript.exe`, `cscript.exe`, `mshta.exe`, and untrusted PowerShell usage via AppLocker / WDAC / SRP.  
  - Disable legacy script engines (VBScript, older JScript) where possible.  
- **Harden macros & Office**:  
  - Disable macros from the internet by default and enforce signed macros for internal use.  
  - Educate users about macro-enabled file types (`.docm`, `.xlsm`) and suspicious prompts.  
- **PowerShell security**:  
  - Enforce Constrained Language Mode where appropriate.  
  - Enable script block and transcription logging; forward logs to the SIEM and alert on suspicious `IEX`, `WebClient`, and encoded commands.  
- **Monitor LOLBIN usage**:  
  - Alert on unusual `mshta.exe` executions, especially with remote URLs.  
  - Track abnormal use of `wscript.exe` / `cscript.exe` starting from email clients or office apps.  
- **Delivery hardening**:  
  - Strengthen email (SPF/DKIM/DMARC, attachment filtering, sandboxing).  
  - Use web filtering and SSL inspection for suspicious domains and HTA downloads.  
  - Control USB usage and block unauthorized HID-like devices (Rubber Ducky, O.MG cable).  

---

## 7) Lessons learned
- **Too many tools? Think in building blocks.**  
  Instead of seeing WSH, HTA, VBA, PowerShell as separate worlds, it’s easier to think in four blocks:  
  - **A: Execute local command** (`cmd.exe`, `powershell.exe`, `calc.exe`…)  
  - **B: Download & execute** (e.g., `WebClient` + `IEX`, HTA-PSH)  
  - **C: Establish reverse shell** (PowerCat, msfvenom/C2 implants)  
  - **D: Auto-execution trigger** (macro autorun, mshta, WSH start, scheduled tasks)  
- **WSH (VBScript)**: can spawn processes, chain commands, and even hide in `.txt` files via `/e:VBScript`.  
- **HTA + mshta**: turns HTML into a fully privileged app; perfect for **remote execution** when a user just needs to click “Run”.  
- **VBA macros**: are still extremely relevant — they provide a natural social-engineering vector (“please open this report”) plus automatic execution hooks.  
- **PowerShell**:  
  - Execution policy is *advisory*, not a real barrier.  
  - It’s ideal for **in-memory payloads** (download + exec) and reverse shells without touching disk.  
- **C2 + delivery**: weaponization is about **connecting these primitives** to C2 and a believable delivery method, not about memorizing every tool’s syntax.  

---

## 8) Links / Resources
- Room: *TryHackMe — Red Team: Weaponization*  
- GitHub: **Red-Teaming-Toolkit (Payload Development section)**  
  - https://github.com/infosecn1nja/Red-Teaming-Toolkit#payload-development  
- Further reading / practice:  
  - THM: Empire, Metasploit, and C2-related rooms.  
  - Microsoft docs on PowerShell logging and Constrained Language Mode.  
  - Office macro security and hardening guidelines.  

---