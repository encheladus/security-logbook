# Inside a Computer System — TryHackMe (Computer Science)

**Date:** 2026-02-19  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This classroom room introduces the fundamental components of a computer system and explains how they interact.  
The objective was to understand the hardware architecture of a computer and the complete boot process from power-on to operating system loading.

---

## 2) Initial hypothesis

Before starting, I expected to:

- Identify the main hardware components of a computer.
- Understand their individual roles.
- Clearly visualize how the boot sequence works.
- Distinguish between firmware, bootloader, and operating system.

---

## 3) Tools used

- TryHackMe platform  
- Local system commands (system information utilities)

---

## 4) Approach (high level)

I approached this room conceptually rather than technically:

- Studied each core hardware component and its function.
- Analyzed how components communicate through the motherboard.
- Broke down the boot process into sequential logical stages.
- Mapped the control flow from hardware to firmware to bootloader to OS.
- Reinforced understanding by checking system information locally.

---

## 5) Results / Evidence (sanitized)

By the end of the room, I was able to:

- Clearly identify all major hardware components (CPU, RAM, storage, motherboard, PSU, network adapter, I/O devices).
- Explain the difference between volatile and persistent memory.
- Describe the full boot sequence step by step.
- Distinguish the roles of UEFI/BIOS, POST, bootloader, and operating system.
- Successfully complete the room and retrieve the flags.

No technical exploitation was involved; this was a foundational theoretical lab.

---

## 6) Recommended remediation

Although not a vulnerability-focused room, from a defensive perspective:

- Keep firmware (UEFI/BIOS) updated to patch security flaws.
- Enable Secure Boot where appropriate.
- Protect boot order settings with firmware passwords.
- Restrict physical access to prevent hardware-level attacks.
- Monitor hardware integrity and system logs for anomalies.

---

## 7) Lessons learned

- A computer system is a coordinated set of hardware components working together to process, store, and transmit data.
- The motherboard acts as the communication backbone.
- The CPU executes instructions and performs computations.
- RAM provides fast but volatile working memory.
- HDD/SSD provides persistent storage.
- The PSU ensures stable power delivery.
- The network adapter enables communication with external systems.
- The boot process follows a strict chain: Power → Firmware → POST → Boot device selection → Bootloader → Operating System.
- Understanding hardware fundamentals strengthens cybersecurity knowledge, especially for low-level attacks and system behavior analysis.

---

## 8) Links / Resources

- Room: TryHackMe — Inside a Computer System  
- Official documentation: UEFI specification, BIOS overview  
- Personal detailed notes: (to be added if published)

---