# Windows x64 Assembly — TryHackMe
**Date:** 2026-01-27  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context
Short summary (1–3 lines): Introduction to x64 Assembly on Windows. This room covers the basics of reverse engineering, assembly instructions, CPU architecture, and Windows x64 calling conventions.

## 2) Initial hypothesis
Expected to understand the fundamentals of reverse engineering and assembly, including registers, memory layout, instructions, and calling conventions. Anticipated initial difficulty in translating high-level logic to low-level execution.

## 3) Tools used
- IDA / Ghidra / Binary Ninja (for analysis)  
- Debuggers (x64dbg, OllyDbg)  
- Text editor / Notes for mapping instructions

## 4) Approach (high level)
- Review x64 architecture and number systems (binary, hexadecimal)  
- Learn data types, bits/bytes, and register usage  
- Study instruction types: data movement, arithmetic, flow control  
- Analyze stack, heap, and program memory layout  
- Understand Windows x64 calling convention and function parameters  
- Methodically step through assembly examples, mapping back to C/C++ concepts  
- Track registers, stack, and memory manually to verify instruction effects  

## 5) Results / Evidence (sanitized)
- Successfully followed instruction flow one step at a time  
- Tracked registers (RAX, RBX, RCX…) and stack usage across function calls  
- Correctly mapped assembly instructions to equivalent high-level constructs  
- Understood how signed vs unsigned comparisons affect conditional jumps  
- Applied Windows x64 calling conventions to determine parameter locations and return values  
- Gained confidence in interpreting memory addresses, offsets, and pointer manipulations  

## 6) Recommended remediation
- Practice consistently with small binaries before attempting complex programs  
- Always separate values vs addresses, and signed vs unsigned comparisons  
- Use diagrams for stack and heap layouts to visualize memory flow  
- Double-check instruction operands and sizes to avoid confusion in execution analysis  
- Maintain a strict step-by-step workflow instead of assuming high-level structure  

## 7) Lessons learned
- Reverse engineering is about reasoning over **state transitions**, not just “reading code”  
- Assembly instructions are individually simple; complexity arises from interactions  
- Calling conventions and compiler optimizations drastically affect control flow interpretation  
- Registers are **temporary execution tools**, not high-level variables  
- Memory layout (stack, heap, segments) is critical for accurate analysis  
- Discipline and methodical tracking overcome initial confusion and cognitive load  

## 8) Links / Resources
- [TryHackMe – Reverse Engineering Room](https://tryhackme.com/room/reverseengineering)  
- [x64 Assembly Guide](https://www.felixcloutier.com/x86/)  
- [Windows x64 Calling Convention Reference](https://docs.microsoft.com/en-us/cpp/build/x64-calling-convention)  
- Notion write-up (public)
