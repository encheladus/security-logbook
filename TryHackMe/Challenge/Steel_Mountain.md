# Steel Mountain — TryHackMe

**Date:** 2026-03-12  
**Type:** TryHackMe Lab  
**Scope:** Lab / Authorized only  

---

## 1) Context

This lab focuses on compromising a Windows machine themed around Mr. Robot.  
The objective is to gain initial access through a vulnerable web service, then perform Windows privilege escalation using enumeration tools and service misconfigurations.

---

## 2) Initial hypothesis

The exposed web application could reveal useful reconnaissance information such as usernames or services.  
A secondary attack surface was expected on non‑standard ports that might expose vulnerable services leading to remote code execution.

---

## 3) Tools used

- Nmap  
- Metasploit  
- PowerShell  
- PowerUp  
- winPEAS  
- Netcat  
- msfvenom  

---

## 4) Approach (high level)

The attack followed a typical Windows exploitation workflow:

- Reconnaissance of the website to identify potential usernames.
- Network enumeration to discover open ports and running services.
- Identification of a vulnerable HTTP service running on an alternative port.
- Exploitation of the vulnerable service to gain initial access.
- Post‑exploitation enumeration using Windows privilege escalation tools.
- Identification of a misconfigured service running with elevated privileges.
- Replacement of the service binary with a malicious payload.
- Restarting the service to execute the payload with SYSTEM privileges.

A second approach involved completing the exploitation manually without automated frameworks by using PowerShell, winPEAS, and manually triggering the vulnerability.

---

## 5) Results / Evidence (sanitized)

The web server hosted **Rejetto HTTP File Server 2.3**, which is vulnerable to remote code execution through **CVE‑2014‑6287**.  
This allowed remote command execution and provided an initial shell on the target machine.

Post‑exploitation enumeration revealed a **misconfigured Windows service**:

- The service ran as **LocalSystem**
- The service binary was **modifiable by the current user**
- The service could be restarted

Because the executable file was writable by a low‑privileged user, it was possible to replace the binary with a malicious executable.  
When the service restarted, it executed the attacker‑controlled payload and returned a shell with **SYSTEM privileges**.

Final state: full administrative access obtained on the machine.

---

## 6) Recommended remediation

To prevent this type of compromise:

- Ensure **service executables are not writable** by non‑administrative users.
- Avoid running services with **LocalSystem privileges** unless absolutely required.
- Apply **strict file system permissions** on service directories.
- Use **fully quoted service paths** to prevent path exploitation.
- Implement **regular patch management** to mitigate known vulnerabilities such as outdated web services.
- Monitor and restrict **service restart permissions** to privileged users only.

---

## 7) Lessons learned

- Public website content can leak **useful reconnaissance information**, such as employee names that may correspond to system usernames.
- Vulnerable web services can provide a **quick foothold** through known remote code execution vulnerabilities.
- Post‑exploitation enumeration tools are extremely valuable for quickly identifying privilege escalation paths.
- **Windows service misconfigurations** are a common and powerful privilege escalation vector.
- Writable service binaries running with high privileges allow **arbitrary code execution as SYSTEM**.
- Understanding how Windows services operate (start, stop, permissions) is critical during post‑exploitation.

---

## 8) Links / Resources

- TryHackMe Room: Steel Mountain  
- CVE: CVE‑2014‑6287 (Rejetto HTTP File Server RCE)  
- PowerUp PowerShell Privilege Escalation Tool  
- winPEAS Privilege Escalation Enumeration Tool  

---