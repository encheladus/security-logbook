# Virtualisation Basics — TryHackMe (Computer Science)

**Date:** 2026-02-22  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room introduced the fundamentals of virtualization and explained why it is a core technology in modern IT infrastructures.  
The objective was to understand how virtualization improves hardware efficiency, enables isolation, and powers cloud computing and cybersecurity labs.

---

## 2) Initial hypothesis

Before starting, I expected to:

- Understand why running one application per physical server is inefficient.
- Learn how virtualization optimizes hardware usage.
- Clearly distinguish between hypervisors, virtual machines, and containers.
- Understand why virtualization is essential in cybersecurity environments.

---

## 3) Tools used

- TryHackMe platform (theoretical classroom room)
- Personal virtual lab knowledge (VirtualBox / VMware concepts)
- Basic system and container management commands (conceptual)

---

## 4) Approach (high level)

- Studied the historical model of “one server = one application”.
- Identified the limitations of physical-only infrastructures (cost, scalability, underutilization).
- Analyzed how hypervisors abstract hardware resources.
- Compared Type 1 vs Type 2 hypervisors.
- Differentiated Virtual Machines from Containers by identifying where isolation occurs.
- Built a layered mental model of architecture:
  - Hardware → Hypervisor → VM → OS → Applications
  - Hardware → Host OS → Container Engine → Containers → Applications
- Used analogies (building/apartment model) to clarify isolation levels.

---

## 5) Results / Evidence (sanitized)

Key conceptual outcomes:

- Clear distinction between physical servers and virtual machines.
- Clear understanding of the role of the hypervisor as the hardware abstraction layer.
- Identification of the main difference between VMs and containers:
  - VMs virtualize hardware and run full operating systems.
  - Containers share the host kernel and isolate applications only.
- Understanding that virtualization significantly increases hardware utilization and reduces operational costs.
- Recognition that most modern infrastructures (cloud, labs, enterprise servers) rely on virtualization.

No technical exploitation was involved in this room, as it was theory-focused.

---

## 6) Recommended remediation

From a defensive and infrastructure perspective:

- Use Type 1 hypervisors in production environments for better performance and security.
- Enforce strict isolation between virtual machines.
- Apply proper network segmentation between VMs and containers.
- Limit container privileges and avoid running containers as root.
- Keep hypervisors, host systems, and container engines updated.
- Monitor exposed services and open ports inside virtual environments.

---

## 7) Lessons learned

- Virtualization solves the inefficiency of dedicating one physical server per application.
- A hypervisor abstracts hardware and enables multiple isolated virtual machines.
- Type 1 hypervisors run directly on hardware; Type 2 run on top of a host OS.
- Virtual Machines emulate full systems with their own OS.
- Containers isolate applications but share the host kernel.
- Containers are lighter and faster but slightly less isolated than VMs.
- Isolation is a critical concept in cybersecurity, especially for malware analysis and lab environments.
- Most real-world targets in bug bounty and pentesting are hosted in virtualized infrastructures.

---

## 8) Links / Resources

- Room: TryHackMe – Computer Science – Virtualisation Basics  
- Official documentation:
  - Hypervisor documentation (vendor-specific)
  - Docker documentation
- Personal Notion notes (public version if published)

---