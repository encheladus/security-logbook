# Nmap - Pentester Tools — TryHackMe

**Date:** 2025-12-13  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This lab focuses on **network enumeration and port scanning fundamentals**, with an emphasis on **Nmap**.  
The learning objective is to understand how scan results are produced, interpreted, and influenced by **firewalls, protocols, and environment constraints**, before any exploitation attempt.

---

## 2) Initial hypothesis

A structured, in-depth exploration of **Nmap scanning techniques** would reveal how different scan types behave, what information they truly provide, and how defensive controls can distort or hide reality during enumeration.

---

## 3) Tools used

- Nmap  
- Nmap Scripting Engine (NSE)  
- Linux terminal environment (TryHackMe Attack Box)  

---

## 4) Approach (high level)

- Start with the principle that **enumeration precedes exploitation**.
- Analyze the concept of **attack surface** through open ports and exposed services.
- Study how TCP and UDP behave at the protocol level.
- Compare common Nmap scan types:
  - TCP Connect (`-sT`)
  - SYN / half-open (`-sS`)
  - UDP (`-sU`)
  - NULL / FIN / Xmas scans
- Examine **host discovery** techniques (ICMP, ARP, TCP probes).
- Evaluate the role of **firewalls and IDS/IPS** in altering scan results.
- Explore **NSE scripts**, their categories, and operational risks.
- Validate findings by correlating multiple scan techniques instead of relying on a single output.

---

## 5) Results / Evidence (sanitized)

- Confirmed that:
  - Hosts can appear **offline** due to ICMP blocking.
  - Ports may appear **closed** when they are actually **filtered**.
- Observed that:
  - SYN scans provide the best balance of speed, accuracy, and noise.
  - UDP scans frequently return **open|filtered**, requiring cautious interpretation.
  - Exotic scan types (NULL, FIN, Xmas) are unreliable on modern systems.
- Identified that firewall behavior itself leaks information and becomes part of the attack surface.

---

## 6) Recommended remediation

- Enforce strict and consistent firewall rules (avoid misleading resets).
- Monitor and alert on abnormal scan patterns (SYN floods, malformed packets).
- Limit service exposure to required ports only.
- Implement IDS/IPS capable of detecting stealth and fragmented scans.
- Regularly audit exposed services and firewall behavior.

---

## 7) Lessons learned

- Enumeration is about **interpreting signals**, not running commands.
- Scan results are **context-dependent**, not absolute truth.
- Firewalls can:
  - Hide hosts
  - Mask open ports
  - Intentionally mislead scanners
- SYN scans are the industry standard when privileges allow.
- UDP scanning is essential but slow and ambiguous.
- NSE is powerful but must be used selectively and responsibly.

---

## 8) Links / Resources

- TryHackMe – Pentester Tools room  
- Nmap official documentation  
- Nmap NSE script reference  
- Public write-up (Notion / GitHub – if published)

---