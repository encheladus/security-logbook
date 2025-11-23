# The Lay of the Land — TryHackMe / Red Team  
**Date:** 2025-11-23  
**Type:** Classroom  
**Scope:** Lab / Authorized only  

---

## 1) Context

Hands-on introduction to common technologies and security products used in corporate environments.  
Focus: understanding network layout, Active Directory structure, users/groups, and both host- and network-based defenses after initial access.

---

## 2) Initial Hypothesis

- The environment uses segmented internal networks and possibly a DMZ.  
- Active Directory is the backbone for identities, groups, and access control.  
- Multiple defensive layers exist: AV/Defender, EDR, host firewall, logging, Sysmon, SIEM, IDS/IPS.  
- Installed applications, services, and shares may expose additional attack surface and credentials.

---

## 3) Tools Used

- Windows built-in commands (systeminfo, netstat, ARP, service & process inspection)  
- PowerShell (network, firewall, AD, logging, process and service enumeration)  
- Windows Management Instrumentation (WMI/WMIC)  
- Active Directory PowerShell cmdlets (user/group/computer/DC enumeration)  
- Event log inspection utilities  
- Sysinternals / Sysmon (awareness and basic checks)

*(No options, credentials, or sensitive scripts included.)*

---

## 4) Approach (High Level)

1. **Locate the host in the network:**  
   Identified network segment, routing, and nearby systems to understand whether the machine sits in an internal VLAN or DMZ, and what it can reach.

2. **Map Active Directory presence:**  
   Checked if the host is domain-joined, then enumerated domain, OUs, domain controllers, users, groups, and high-value accounts.

3. **Profile users and groups:**  
   Distinguished local vs domain accounts, reviewed privileged groups (Domain Admins, Enterprise Admins, etc.) and potential escalation paths.

4. **Enumerate host-based defenses:**  
   Identified antivirus, Microsoft Defender status, host firewall profiles, event logging, and potential Sysmon/EDR presence to understand local visibility.

5. **Identify network security controls:**  
   Considered how firewalls, SIEM, and NIDS/NIPS would see scans, authentication attempts, and lateral movement from this foothold.

6. **Review applications and services:**  
   Listed installed software, running services/processes, shared resources, and internal-only services (web apps, DNS, file shares) to spot possible misconfigurations and new targets.

---

## 5) Results / Evidence (Sanitized)

- Confirmed the compromised host is part of an internal, segmented corporate network rather than directly exposed to the Internet.  
- Verified that the machine is joined to an Active Directory domain, with clear hierarchy of users, groups, and domain controllers.  
- Identified multiple privileged groups (e.g., Domain Admins, Enterprise Admins, Server Operators) with different impact levels for escalation.  
- Observed multiple host-based security components (AV/Defender, firewall, logging, potentially Sysmon/EDR), indicating significant endpoint visibility.  
- Recognized network-level defenses (firewalls, SIEM, IDS/IPS) that would likely record or alert on noisy activity.  
- Saw evidence of typical corporate software, shares, and internal services that could become pivot points or data sources in later stages.

*(No real hostnames, IPs, usernames, or logs included.)*

---

## 6) Recommended Remediation

- **Network architecture**
  - Maintain strict segmentation (VLANs, DMZs) with least-privilege routing between segments.  
  - Enforce strong access controls between user networks, server networks, and management zones.

- **Active Directory hygiene**
  - Minimize membership in highly privileged groups; apply tiered admin models.  
  - Harden domain controllers and restrict interactive logons.  
  - Regularly audit service accounts for over-privilege and stale credentials.

- **Host-based defenses**
  - Ensure AV/Defender, Sysmon/EDR, and host firewalls are consistently deployed and centrally managed.  
  - Use hardening baselines and application allowlisting where possible.

- **Network security & monitoring**
  - Tune SIEM rules and IDS/IPS signatures to detect lateral movement, abnormal AD queries, and suspicious process behavior.  
  - Correlate endpoint and network telemetry for more accurate detection.

- **Configuration & patching**
  - Keep applications and services patched and remove unused or legacy software.  
  - Review file and share permissions to prevent unnecessary exposure of sensitive data.

---

## 7) Lessons Learned

- **Initial shells are only an entry point.**  
  Real value comes from understanding where you are in the network, how AD is structured, and what’s defending the environment.

- **Segmentation shapes your attack path.**  
  VLANs, internal networks, and DMZs control what you can see and where you can pivot.

- **Active Directory defines power.**  
  Domains, OUs, DCs, and privileged groups dictate who truly “owns” the environment; enumeration here is mandatory.

- **Defense is layered.**  
  Host-based controls (AV, Defender, Sysmon, EDR, firewalls) and network controls (firewalls, SIEM, IDS/IPS) overlap to provide strong visibility.

- **Enumeration must be intentional.**  
  Every recon action has a detection cost; understanding blue team tools lets you balance information gathering with stealth.

---

## 8) Links / Resources

- Room / machine: *TryHackMe — Red Team (Corporate Environment / Security Products module)*  
- Microsoft Docs – Active Directory overview  
- Microsoft Docs – Windows Defender / Microsoft Defender for Endpoint  
- Sysinternals – Sysmon and related tools  
- General SIEM & IDS/IPS vendor documentation (Splunk, Suricata, etc.)  
- Notion / detailed write-up (internal): *add your link here*

---