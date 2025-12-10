# Firewalls — TryHackMe - Red Team  
**Date:** 2025-12-10  
**Type:** TryHackMe / Classroom  
**Scope:** Lab / Authorized only  

---

## 1) Context

Learn and experiment with multiple firewall evasion techniques such as port hopping, tunneling, fragmentation, header manipulation, and spoofing.  
Objective: understand how modern and legacy firewalls inspect traffic and how attackers bypass weak filtering.

---

## 2) Initial hypothesis

- Identify and compare different types of firewalls.  
- Evaluate which evasion techniques might bypass stateless or weak filtering logic.  

---

## 3) Tools used

- Nmap  
- Ncat  
- TCP/IP utilities (baseline network inspection)  

*No credentials, payloads, or sensitive scripts included.*

---

## 4) Approach (high level)

- Reviewed firewall classifications by model, scale, and inspection capabilities.  
- Tested evasion techniques by manipulating source MAC/IP, ports, and packet headers.  
- Applied fragmentation (fixed MTU, 8-byte, 16-byte) and payload padding to alter packet structures.  
- Used port hopping and port tunneling concepts to bypass port-based filtering.  
- Compared limitations of traditional firewalls vs. NGFW behavior.  

---

## 5) Results / Evidence (sanitized)

- Spoofing or altering origin metadata (MAC/IP/ports) allowed certain scans to pass simple filtering.  
- Fragmented packets and low-level header manipulation bypassed stateless filtering but failed against stateful inspection.  
- Port hopping successfully located allowed outbound ports; tunneling worked when intermediate hosts permitted forwarding.  
- Some scans using spoofed IP returned **no usable results**, confirming that responses cannot be received unless the spoofed address is reachable or controlled.  
- NGFW-like behavior (payload awareness, DPI) blocked most evasion attempts regardless of port or fragmentation tricks.  

---

## 6) Recommended remediation

- Enforce **stateful inspection** and discard out‑of‑state packets.  
- Implement **DPI (Deep Packet Inspection)** to detect spoofed, fragmented, or malformed traffic.  
- Require **idempotency and strict routing checks** for allowed services.  
- Enforce **MAC/IP binding**, anti-spoofing filters, and ingress/egress validation.  
- Apply **application-layer controls** instead of port-based filtering alone.  

---

## 7) Lessons learned

- Port‑based filtering alone is insufficient for modern threats.  
- Many evasion techniques rely on weaknesses in stateless firewall logic.  
- Spoofing IP headers is ineffective unless replies are capturable.  
- Fragmentation and header manipulation still bypass outdated devices.  
- NGFWs drastically reduce the surface for these evasion techniques.  

---

## 8) Links / Resources

- TryHackMe Room: Red Team – Firewall Evasion  
- Nmap documentation (official)  
- Personal Notion notes (public version)

---