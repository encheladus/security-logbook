# Intro to Containerisation — TryHackMe

**Date:** 2026-01-26  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Introductory classroom lab on containerisation.  
Covers what containers are, why they are used, and how Docker implements containerisation on top of Linux kernel features.

---

## 2) Initial hypothesis

Before starting, I expected containerisation to:
- Provide strong isolation similar to virtual machines
- Act as a security boundary by default
- Be mostly abstracted away by Docker tooling

I also assumed containers behaved like lightweight operating systems rather than isolated processes.

---

## 3) Tools used

- TryHackMe classroom environment  
- Docker (conceptual exposure)

---

## 4) Approach (high level)

- Study the definition and goals of containerisation
- Compare containers vs virtual machines conceptually
- Understand Docker’s role as a container runtime and tooling layer
- Analyze how Linux namespaces enable isolation
- Examine process hierarchy (PID namespaces) to understand container behavior
- Connect isolation mechanisms to real security implications

---

## 5) Results / Evidence (sanitized)

Observed that:
- Containers package applications and dependencies, **not operating systems**
- Isolation is achieved through **Linux namespaces**, not hardware virtualization
- Containers share the **host kernel**, making them lightweight but not inherently secure
- Containers can have their own **PID 1**, creating the illusion of an independent system
- Misconfigured namespaces may allow **container escape** scenarios

Final state: clear conceptual model of containerisation internals and limitations.

---

## 6) Recommended remediation

From a defensive and architectural standpoint:
- Treat containers as **process isolation**, not sandboxing
- Apply strict namespace separation and avoid host namespace sharing
- Enforce least privilege and capability dropping
- Use additional security layers (seccomp, AppArmor, SELinux)
- Monitor and harden container runtime configurations
- Apply defense-in-depth rather than trusting Docker isolation alone

---

## 7) Lessons learned

- Containerisation bundles an application **and its environment**, not a full OS
- Containers are lightweight because they **share the host kernel**
- Docker Engine manages build, runtime, and orchestration — it is not a security boundary
- **Namespaces** are the core isolation primitive
- A container’s PID 1 exists only within its namespace
- Containers are **not secure by default**
- Namespace misconfiguration can lead to **container escapes**, critical for pentesting

---

## 8) Links / Resources

- TryHackMe — Intro to Containerisation  
- Linux namespaces documentation  
- Docker official documentation  
- Public notes / detailed write-up (if published later)

---
