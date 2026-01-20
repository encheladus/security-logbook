# iOS Forensics — TryHackMe - Forensics

**Date:** 2026-01-20  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Introduction to iOS digital forensics, focusing on data acquisition methods, iOS file systems, security mechanisms, and forensic artefact analysis.  
Objective: understand how forensic investigators extract, interpret, and preserve evidence from iOS devices.

---

## 2) Initial hypothesis

I expected to learn the fundamentals of iOS forensics, including how data is stored, protected, and retrieved, and how modern iOS security impacts forensic investigations.

---

## 3) Tools used

- DB Browser for SQLite  
- Hex editor (e.g. HxD)  
- iTunes (conceptual use for backups)  
- Cellebrite (theoretical / lab context)  

---

## 4) Approach (high level)

- Review core digital forensics principles (evidence integrity, artefacts, timelines).  
- Study challenges faced by forensic analysts (encryption, scale, cost, attribution).  
- Analyze iOS file systems (HFS+ vs APFS) and their forensic implications.  
- Examine modern iOS security features and their impact on acquisition.  
- Explore iPhone data acquisition methods (direct, logical, advanced logical, physical).  
- Identify common iOS artefacts (plist files, SQLite databases).  
- Assess risks and best practices when handling iOS devices.

---

## 5) Results / Evidence (sanitized)

- Identified that iOS devices store extensive forensic artefacts beyond visible files (plists, databases, backups).  
- Observed that many plist files are stored in binary format and require specialized tools to interpret.  
- Confirmed that logical and backup-based acquisition is often safer and more practical than physical acquisition.  
- Verified that trust certificates enable privileged access when devices have been previously paired.  
- Noted that modern iOS security (encryption, Restricted Mode) significantly limits forensic access without credentials.

---

## 6) Recommended remediation

- Use logical or advanced logical acquisition when possible to reduce forensic risk.  
- Enforce strict chain of custody and documentation procedures.  
- Avoid powering off or isolating devices improperly to prevent data loss or remote wipe.  
- Apply strong encryption awareness when planning forensic strategies.  
- Rely on database and plist analysis tools for structured artefact examination.

---

## 7) Lessons learned

- Digital forensics depends as much on methodology and documentation as on technical skill.  
- iOS security improvements benefit users but complicate forensic investigations.  
- Trust relationships (paired computers, certificates) are critical forensic leverage points.  
- Missing or wiped data can itself be forensic evidence.  
- iPhones function as dense behavioural logs of user activity.

---

## 8) Links / Resources

- TryHackMe Room: iOS Forensics  
- ACPO Good Practice Guide for Digital Evidence  
- Apple File System (APFS) documentation  
- Public iOS forensic analysis guides  

---
