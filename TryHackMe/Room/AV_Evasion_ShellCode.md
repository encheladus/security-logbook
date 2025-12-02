# AV Evasion: Shellcode — TryHackMe - Red Team  
**Date:** 2025-12-02  
**Type:** TryHackMe  
**Scope:** Authorized Lab

---

## 1) Context
This lab dives deep into payload construction and AV evasion techniques:

- How shellcodes are created  
- How PE files are structured and loaded  
- How to encode, encrypt, pack, and bind payloads  
- How to build custom loaders and stagers  
- How to evade static and dynamic detection engines

Goal: understand why public payloads are instantly detected, and how to build custom, evasive payload chains.

---

## 2) Starting hypothesis
To evade AV/EDR:

- Public shellcodes (Metasploit, Cobalt Strike) are known → must be modified.  
- Built-in encoders aren’t enough → need custom encoding/encryption.  
- PE structure matters → determines where malicious code can hide.  
- Staged payloads offer stealth and agility; stageless are simpler but noisy.  
- Packers and binders have pros/cons; they don’t magically bypass AV.

---

## 3) Method / Used Tools

### ✔ Deep dive into the PE structure
Studied the role of core sections:

- `.text` → instructions  
- `.data` / `.bss` → variables  
- `.idata` / `.edata` → imports & exports  
- `.reloc` → ASLR & relocations  
- `.rsrc` → icons, manifests, embedded content  

Tools used:  
- **PE-Bear** to visualize headers, sections, and anomalies.

### ✔ Shellcode fundamentals
- Assembly → opcodes → extraction → shellcode  
- Jump-call-pop patterns  
- Syscalls  
- Register usage  
- Position-independent code  

Built and extracted shellcode manually using:
- `nasm`, `ld`, `objcopy`, `xxd`.

### ✔ Generating shellcode (Metasploit)
- Used `msfvenom` to generate staged & stageless payloads.  
- Observed that **public payloads = AV instant detection**.

### ✔ Staged vs Stageless
**Stageless:**  
- All-in-one binary  
- Big, noisy, detectable  

**Staged:**  
- Small stage0 stager  
- Downloads/encrypts/decrypts stage2 in memory  
- Memory-only execution  
- AV has fewer static indicators

### ✔ Custom stager in C#
- `WebClient` download → decode → `VirtualAlloc` → `Marshal.Copy` → `CreateThread`  
- This is the foundation of many red-team loaders.

### ✔ Encoding vs Encryption
Encoding:
- XOR, Base64, hex → easy to reverse  
- Breaks superficial signatures but AV can decode

Encryption:
- AES, RC4, XOR+key → requires secret key  
- Looks random → defeats static scanners  
- Needs embedded decryptor logic in dropper

### ✔ Custom encoding pipeline
1. Generate shellcode  
2. XOR encrypt with custom key  
3. Base64 encode  
4. Write dropper to decode+decrypt  
5. Execute in RWX memory  
6. Obfuscate dropper using ConfuserEx  
7. (Optional) Bind with legitimate EXE

### ✔ Packers
- Used **ConfuserEx** to pack .NET shells  
- Observed:
  - AV misses packed file on disk  
  - But catches behavior once payload executes (memory scanning / API hooks)

### ✔ Binders
- Used `msfvenom -x legitimate.exe` to bind EXEs  
- Confirmed:  
  → **Binders fool users, not AV.**  
  → Payload still raw → still detected.

---

## 4) Issue met

- **Public payloads = 100% detected**, even packed or encoded  
- Metasploit encoders (Shikata, etc.) = automatically decoded by AV  
- Packers (UPX, ConfuserEx) only help on disk, not in memory  
- Binders don’t help at all for detection → only for social engineering  
- Reverse shells trip dynamic detection immediately (CreateProcess, RWX pages, suspicious network traffic)

Conclusion:  
**Everything public is burned. Everything known is detected. Everything predictable is analyzed.**

---

## 5) Solution

### ✔ Build a fully custom encoding layer  
- XOR + Base64  
  → Unrecognized by AV  
  → No matching signatures  
  → Payload looks like harmless random data

### ✔ Use a custom C# dropper
- Decode → XOR decrypt → `VirtualAlloc` → `CreateThread`  
- Shellcode only exists in memory  
- Minimal on-disk footprint

### ✔ Obfuscate the loader
Used **ConfuserEx** to obfuscate:

- Control-flow  
- Symbols  
- IL patterns  
- Strings & metadata  

→ Breaks static analysis  
→ Hardens the unpacking stub

### ✔ Binder only for delivery
Once the dropper is obfuscated:

- Bind with legitimate EXE  
- User sees real program  
- Malicious part runs silently  
- Binder ≠ AV evasion, but great for deception

---

## 6) What I have learned

- **PE format** knowledge is mandatory for any serious payload work.  
- Public shellcode is **detected instantly**—must be custom or at least transformed.  
- **Encoding ≠ encryption**:  
  - encoding breaks signatures  
  - encryption hides content  
- Packers only hide disk artifacts → AV still catches runtime behavior.  
- Binders are for *human deception*, not AV evasion.  
- A proper payload chain combines:
  - custom encoding  
  - encryption  
  - obfuscated loader  
  - staged architecture  
  - careful API usage  
  - clean delivery mechanism  
- True evasion comes from **custom pipelines**, not public tooling.

---

## 7) Takeaways

- Stop relying on public payloads and public encoders.  
- Build **your own encoding + your own loader**.  
- For stealth: use **staged**, memory-only execution.  
- Obfuscate everything: code, strings, control-flow.  
- Packers help disk evasion, not runtime.  
- Binders only hide intent from the *user*, not the *defender*.  
- Evasion is a **chain**, not a trick.

---