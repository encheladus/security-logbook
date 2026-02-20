# Computer Types — TryHackMe (Computer Science)

**Date:** 2026-02-20  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This TryHackMe classroom room explores the different types of computers, from traditional personal machines to embedded and IoT systems.  
The objective was to understand how purpose and design constraints define each system category and why this matters in cybersecurity.

---

## 2) Initial hypothesis

Before starting, I expected to:

- Clearly distinguish between visible systems (laptops, desktops) and less visible ones (servers, IoT, embedded systems).
- Understand what technically differentiates similar categories (e.g., workstation vs desktop, embedded vs IoT).
- Identify how design constraints influence performance, reliability, and attack surface.

---

## 3) Tools used

- TryHackMe classroom material  
- Personal notes  

---

## 4) Approach (high level)

I approached the room conceptually rather than technically:

- Identified each computer type and its primary purpose.
- Compared similar systems to understand subtle differences.
- Analyzed design trade-offs (mobility vs power, reliability vs cost, connectivity vs isolation).
- Reframed classification around operational context instead of physical appearance.

The focus was on understanding *why* systems are built a certain way, not just *what* they are.

---

## 5) Results / Evidence (sanitized)

I can now confidently differentiate between:

- **Laptops** (portable, battery-optimized, general-purpose).
- **Desktops** (stationary, upgradeable, better cooling and sustained performance).
- **Workstations** (high-end professional systems for intensive workloads).
- **Servers** (network-oriented machines providing services to multiple clients).
- **Smartphones/Tablets** (compact, energy-efficient full computers).
- **IoT devices** (network-connected, single-purpose systems).
- **Embedded systems** (dedicated controllers integrated into larger machines, possibly offline).

Key clarification achieved:

- Embedded ≠ IoT (network connectivity is the defining difference).
- Server ≠ high-end desktop (purpose and uptime requirements define the server).
- Workstation ≠ desktop (professional-grade reliability and performance focus).

---

## 6) Recommended remediation

From a cybersecurity perspective, security strategy should adapt to system type:

- Apply strict network segmentation for IoT devices.
- Minimize exposed services on servers.
- Use redundancy and monitoring for critical infrastructure.
- Harden embedded systems that cannot easily be patched.
- Reduce unnecessary connectivity to limit attack surface.

Security controls must match the operational context and constraints of each system.

---

## 7) Lessons learned

- A computer is defined by its ability to process instructions, not by having a screen or keyboard.
- Purpose determines architecture.
- Engineering decisions are always trade-offs.
- Portability limits sustained performance.
- Reliability increases cost.
- Connectivity increases exposure.
- Every connected device represents a potential attack surface.

Understanding system categories improves threat modeling and attack surface mapping in pentesting.

---

## 8) Links / Resources

- Room: TryHackMe – Computer Science (Computer Types)
- Personal detailed notes (Notion)