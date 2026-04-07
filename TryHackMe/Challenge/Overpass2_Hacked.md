# Overpass 2 - Hacked — TryHackMe

**Date:** 2026-04-02  
**Type:** TryHackMe  
**Scope:** Lab  

---

## 1) Context

Analysis of a compromised system using a captured PCAP file.  
Objective: understand the attacker’s entry point, recover access, and fully compromise the machine.

---

## 2) Initial hypothesis

- Attacker likely gained access through a web endpoint
- Presence of command injection or file upload leading to remote execution
- Possible persistence mechanism and privilege escalation vector on the system

---

## 3) Tools used

- Wireshark  
- SSH  
- Linux CLI  

---

## 4) Approach (high level)

- Analyze network traffic (PCAP) to identify suspicious activity  
- Reconstruct attacker payloads via HTTP stream inspection  
- Extract credentials and sensitive data from captured traffic  
- Investigate persistence mechanisms (backdoor analysis)  
- Use recovered credentials to access the target system  
- Perform local enumeration for privilege escalation vectors  
- Exploit misconfigurations to gain root access  

---

## 5) Results / Evidence (sanitized)

- Identified malicious HTTP POST request containing a reverse shell payload  
- Confirmed attacker remote command execution via netcat reverse shell  
- Extracted credentials from network traffic and related artifacts  
- Discovered weak cryptographic implementation (hash + static salt) enabling password recovery  
- Successfully authenticated to the target system via SSH (non-standard port)  
- Gained user-level access and retrieved user flag  
- Identified SUID binary allowing privilege escalation  
- Achieved root access and retrieved root flag  

Final state: full system compromise (user → root)

---

## 6) Recommended remediation

- Sanitize and validate all user inputs on web endpoints  
- Prevent arbitrary command execution (secure coding practices)  
- Avoid hardcoded secrets in source code  
- Use strong password hashing mechanisms with unique salts  
- Restrict and monitor outbound connections to prevent reverse shells  
- Audit file permissions regularly (especially SUID binaries)  
- Enforce least privilege principles  
- Monitor network traffic for suspicious patterns  

---

## 7) Lessons learned

- PCAP analysis can directly lead to initial access, not just detection  
- Reverse shell patterns are identifiable in raw network traffic  
- HTTP stream inspection is critical in incident response  
- Weak cryptographic implementations can be exploited offline  
- Credential reuse and exposure remain a major risk  
- Attack chains combine multiple small weaknesses  
- SUID misconfigurations are a common privilege escalation vector  
- Post-exploitation enumeration is essential for full compromise  

---

## 8) Links / Resources

- TryHackMe Room: Overpass 2 - Hacked  
- Wireshark Documentation  
- Linux Privilege Escalation (SUID binaries)  
- Reverse Shell Cheat Sheets  
