# Vulnversity — TryHackMe

**Date:** 2026-03-07  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

TryHackMe machine focused on learning the full attack chain: reconnaissance, web enumeration, web exploitation, and privilege escalation.  
The objective was to identify exposed services, exploit a vulnerable file upload functionality to obtain a shell, and escalate privileges to root.

---

## 2) Initial hypothesis

The machine likely exposes a web application with hidden functionality that could be discovered through enumeration.  
A common expectation was the presence of a vulnerable upload feature allowing remote code execution, followed by a privilege escalation vector on the host system.

---

## 3) Tools used

- Nmap  
- Gobuster  
- Burp Suite (Intruder)  
- Netcat  

---

## 4) Approach (high level)

The attack followed a typical penetration testing workflow:

First, network reconnaissance was performed to identify open ports and running services on the target machine. This step helps understand the attack surface and determine which services may be vulnerable or misconfigured.

Once a web service was identified, directory and file enumeration was conducted to discover hidden endpoints that were not visible during normal browsing. This process revealed additional application functionality, including an upload feature.

The upload form was then analyzed to determine which file types were accepted by the server. Extension fuzzing was performed to test multiple variations of PHP extensions and identify one that bypassed the server-side filtering.

After identifying a permitted extension, a reverse shell payload was uploaded through the form. Accessing the uploaded file forced the server to execute the script, which initiated a connection back to the attacker machine and provided remote shell access.

With initial access obtained, the next phase involved local enumeration of the system to identify privilege escalation opportunities. A misconfiguration allowed the creation and execution of a custom systemd service, which could run commands with root privileges.

---

## 5) Results / Evidence (sanitized)

The attack chain successfully resulted in remote shell access to the web server through exploitation of the upload functionality.

Impact observed:

- Upload filter bypass allowed execution of server-side code.
- Remote command execution was obtained via a reverse shell.
- A systemd service misconfiguration enabled execution of a command as root.

Final impact:

- Privilege escalation to root by executing a command through a malicious systemd service.
- Sensitive file owned by root was successfully accessed via a writable directory.

---

## 6) Recommended remediation

Several defensive measures could prevent this type of compromise:

- Implement strict server-side file validation for uploads (allowlist extensions and MIME type verification).
- Store uploaded files outside the web root and prevent direct execution.
- Disable execution permissions on upload directories.
- Apply the principle of least privilege to system management tools such as systemctl.
- Restrict the ability of non-privileged users to create or enable system services.
- Implement file system permissions and monitoring to detect suspicious service creation.

---

## 7) Lessons learned

- Reconnaissance is essential to map the attack surface before exploitation.
- Directory brute forcing can reveal hidden functionality in web applications.
- File upload features are high-risk if validation is improperly implemented.
- Extension fuzzing can bypass weak file filtering mechanisms.
- Reverse shells provide reliable remote command execution in controlled environments.
- Misconfigured system management tools can lead to privilege escalation.

---

## 8) Links / Resources

- Room: https://tryhackme.com/room/vulnversity  
- Nmap documentation: https://nmap.org/docs.html  
- Gobuster repository: https://github.com/OJ/gobuster  
- Burp Suite documentation: https://portswigger.net/burp