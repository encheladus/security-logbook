# Red Team IDS/IPS Evasion — TryHackMe
**Date:** 2025-12-09  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context
An Intrusion Detection System (IDS) detects network or system intrusions, while an Intrusion Prevention System (IPS) can detect **and** prevent intrusions. Snort can function as either an IDS or IPS, with IPS mode requiring a mechanism to block (`drop`) offending connections.

IDS setups can be divided based on their network location:

1. **Host-based IDS (HIDS)**: Installed on an OS to monitor host traffic and processes.
2. **Network-based IDS (NIDS)**: Dedicated appliance/server monitoring network traffic or VLANs.

This lab focuses on understanding IDS/IPS detection and evasion techniques.

---

## 2) Initial hypothesis
Learn about and experiment with various IDS/IPS evasion techniques, such as protocol and payload manipulation.

---

## 3) Tools used
- Snort (IDS/IPS engine)  
- Nmap (packet crafting, port manipulation)  
- hping3 (packet manipulation)  
- Socat / OpenSSL (encrypted communication)  
- CyberChef (payload encoding/obfuscation)  

---

## 4) Approach (high level)
- Analyze IDS/IPS detection logic (signature vs anomaly).  
- Identify potential evasion strategies for network traffic.  
- Experiment with protocol manipulation (TCP/UDP, fragmentation, flags).  
- Experiment with payload manipulation (encoding, encryption, syntax changes).  
- Test tactical evasion (DoS, flooding, logging overload).  
- Explore C2 customization (User-Agent, sleep/jitter, DNS/HTTPS channels).  

---

## 5) Results / Evidence (sanitized)
- Signature-based IDS/IPS rules were bypassed when payloads were:
  - Base64 or URL encoded
  - Reordered, spaced differently, or substituted tools
  - Encrypted via socat/OpenSSL
- Manipulating TCP flags (Xmas, Null, SYN/FIN) evaded naive filters.  
- Fragmented or malformed packets reduced detection reliability.  
- Tactical DoS (high volume or edge-case traffic) caused IDS/IPS slowdowns and reduced log visibility.  
- C2 customizations (User-Agent, sleep/jitter) successfully masked beacon traffic.  

---

## 6) Recommended remediation
- Ensure **IDS/IPS rules** are up-to-date and regularly audited.  
- Deploy **Next-Generation IPS/NGFW** with:
  - Full-stack inspection (L2–L7)  
  - Application, content, and context awareness  
  - Rapid signature/rule updates  
- Monitor for **tactical evasion** (traffic floods, fragmented/malformed packets).  
- Implement **end-to-end encryption inspection** or secure channel policies where possible.  
- Consider **anomaly-based detection** to catch unknown attack patterns.  

---

## 7) Lessons learned
1. IDS vs IPS: **detection vs prevention**.  
2. Signature-based detection is precise but **easily evaded**.  
3. Anomaly-based detection is flexible but prone to **false positives**.  
4. Evasion techniques exploit **rigid assumptions** in detection engines.  
5. Modern systems rely on **contextual and content-aware inspection**.  
6. Red team operations benefit from **custom C2 profiles** to avoid alerts.  

---

## 8) Links / Resources
- [TryHackMe Red Team Room](https://tryhackme.com/room/redteam)  
- Snort official documentation: [https://snort.org/documents](https://snort.org/documents)  
- CyberChef: [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/)  
- Notion / detailed write-up: [Public link if available]