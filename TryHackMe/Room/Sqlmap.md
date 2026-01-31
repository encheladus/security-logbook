# Sqlmap — TryHackMe

**Date:** 2026-01-30  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

This classroom-style TryHackMe room introduces **Sqlmap**, an automated SQL injection exploitation tool.  
The objective was to understand how Sqlmap detects SQL injection flaws, enumerates databases, and assesses post-exploitation impact in a controlled environment.

---

## 2) Initial hypothesis

Given that Sqlmap automates SQL injection discovery and exploitation, I expected the lab to demonstrate:
- Detection of injectable parameters with minimal manual effort
- Automated enumeration of database metadata
- Limitations depending on database user privileges

I also assumed default settings would be sufficient for most vulnerabilities.

---

## 3) Tools used

- Sqlmap  
- Web browser (request inspection / parameter identification)

_No credentials, payloads, or sensitive scripts were used._

---

## 4) Approach (high level)

1. Identify potential injectable parameters in GET and POST requests.
2. Configure Sqlmap to target specific parameters instead of blind scanning.
3. Increase testing depth when default detection failed.
4. Confirm SQL injection presence.
5. Enumerate database information incrementally (DB → tables → columns).
6. Assess exploitation limits based on database privileges.
7. Stop escalation attempts when technical or permission boundaries were reached.

---

## 5) Results / Evidence (sanitized)

- SQL injection vulnerabilities were successfully identified on selected parameters.
- Database metadata (DBMS type, current database, table structure) was enumerated.
- In some cases, data extraction was possible.
- OS-level interaction was not always achievable due to restricted database privileges.

_Final state: SQL injection confirmed with partial enumeration; impact constrained by permissions._

---

## 6) Recommended remediation

- Use prepared statements / parameterized queries.
- Enforce strict input validation and sanitization.
- Apply least-privilege principles to database users.
- Disable unnecessary database features and dangerous permissions.
- Monitor and rate-limit abnormal query behavior.
- Implement WAF rules for SQL injection detection.

---

## 7) Lessons learned

- Sqlmap is extremely powerful, but **precision matters more than automation**.
- Default settings prioritize safety over coverage and can miss real issues.
- Understanding HTTP requests and application logic is essential.
- Enumeration should be methodical and controlled.
- SQL injection impact is directly tied to database privilege levels.

---

## 8) Links / Resources

- TryHackMe — Sqlmap room  
- Official Sqlmap documentation  
- Public write-up (Notion → GitHub)

---