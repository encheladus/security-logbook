# Introductory Networking — TryHackMe - Network

**Date:** 2026-01-14  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Introductory Networking (TryHackMe classroom-style lab) covering core networking theory and basic diagnostic tools.  
Learning objective: understand layered networking models (OSI / TCP-IP) and apply foundational tools for network analysis.

---

## 2) Initial hypothesis

Before starting, I expected this lab to focus on:
- Conceptual understanding of the OSI and TCP/IP models
- How these models map to real-world networking behavior
- Basic usage of common networking diagnostic tools (ping, traceroute, DNS queries)

---

## 3) Tools used

- ping  
- traceroute / tracert  
- whois  
- dig  

(No advanced options, automation, or custom scripts used.)

---

## 4) Approach (high level)

- Reviewed the OSI model layer by layer to understand each responsibility.
- Mapped OSI layers to their TCP/IP equivalents to bridge theory and practice.
- Studied encapsulation and de-encapsulation to understand packet flow.
- Used basic command-line tools to observe how networking concepts appear in real output:
  - Connectivity checks
  - Routing paths
  - Domain registration data
  - DNS resolution flow

---

## 5) Results / Evidence (sanitized)

- Confirmed how data changes form across layers (data → segment → packet → frame → bits).
- Observed real routing paths and latency using traceroute.
- Verified DNS resolution flow and TTL behavior with dig.
- Identified how privacy protections affect Whois output.
- Successfully correlated each tool to its relevant OSI / TCP-IP layer(s).

No sensitive data, credentials, or internal infrastructure details were accessed or exposed.

---

## 6) Recommended remediation

*(General networking best practices derived from the lab)*

- Use layered troubleshooting to isolate issues (physical → application).
- Ensure DNS configuration and caching behavior are well understood.
- Monitor routing paths to detect latency or misconfiguration.
- Apply privacy-aware domain registration practices.
- Maintain clear separation between logical addressing (IP) and physical addressing (MAC).

---

## 7) Lessons learned

- The OSI model is primarily a **learning and troubleshooting framework**, while TCP/IP is the **practical implementation**.
- Encapsulation and de-encapsulation are fundamental to interoperability between systems.
- Networking issues often originate at unexpected layers.
- Command-line tools provide essential visibility into otherwise abstract network behavior.
- Understanding DNS resolution deeply is critical for web and infrastructure troubleshooting.

---

## 8) Links / Resources

- TryHackMe Room: Introductory Networking  
- RFC 1122 — TCP/IP Model  
- OSI Model documentation  
- Public DNS and networking tool manuals  

---
