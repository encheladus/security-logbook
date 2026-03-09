# Kenobi — TryHackMe

**Date:** 2026-03-08  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

This lab focuses on exploiting a Linux machine by chaining multiple weaknesses.  
The objective is to practice service enumeration, abuse misconfigured file‑sharing services, exploit a vulnerable FTP service, and perform privilege escalation through PATH manipulation.

---

## 2) Initial hypothesis

Based on the exposed services, the attack surface likely includes file‑sharing protocols and remote access services.

Potential entry points include:

- Anonymous access to SMB shares
- Misconfigured NFS exports allowing remote directory access
- A vulnerable version of ProFTPD
- Possible privilege escalation through misconfigured SUID binaries

The expected attack chain was to gain initial access through a service vulnerability and then escalate privileges locally.

---

## 3) Tools used

- Nmap  
- smbclient  
- smbget  
- Netcat  
- searchsploit  
- SSH  
- Linux built‑in utilities (find, strings)

---

## 4) Approach (high level)

The exploitation process followed a structured enumeration and attack methodology:

1. **Service enumeration**

   - Performed a network scan to identify open ports and running services.
   - Identified several exposed services including FTP, SSH, HTTP, SMB, RPCBind, and NFS.

2. **SMB share enumeration**

   - Attempted automated enumeration with Nmap scripts.
   - Switched to manual enumeration and discovered an anonymously accessible SMB share.

3. **NFS enumeration**

   - Investigated RPCBind and confirmed that the `/var` directory was exported via NFS.
   - This meant the directory could potentially be mounted remotely.

4. **Exploiting ProFTPD**

   - Identified the FTP service version and searched for public exploits.
   - Discovered a vulnerability in the `mod_copy` module allowing arbitrary file copying.
   - Abused this functionality to copy a private SSH key to a location accessible through the NFS share.

5. **Initial access**

   - Mounted the exported NFS share and retrieved the copied SSH key.
   - Used the key to authenticate via SSH and gain shell access as a normal user.

6. **Privilege escalation**

   - Enumerated SUID binaries on the system.
   - Identified a custom SUID binary that executed system commands without absolute paths.
   - Exploited this behavior by manipulating the `PATH` environment variable to force the program to execute a malicious binary, resulting in a root shell.

---

## 5) Results / Evidence (sanitized)

The attack chain successfully led to full system compromise:

- Anonymous SMB access revealed accessible network shares.
- NFS enumeration exposed a remotely mountable `/var` directory.
- A vulnerable FTP module allowed unauthorized copying of a private SSH key.
- The retrieved key enabled SSH authentication as a valid system user.
- A misconfigured SUID binary allowed privilege escalation through PATH hijacking.

Final state:

- Initial user access obtained through SSH authentication.
- Root privileges achieved through execution of a manipulated command path.

---

## 6) Recommended remediation

Several defensive measures can prevent this type of attack chain:

- **Disable anonymous SMB access** unless strictly required.
- **Restrict NFS exports** to trusted hosts and enforce proper access controls.
- **Update or patch vulnerable services**, including outdated FTP server versions.
- **Protect sensitive files**, especially SSH private keys.
- **Avoid custom SUID binaries** unless absolutely necessary.
- Ensure privileged programs use **absolute paths when executing system commands**.
- Implement **regular security audits** to detect misconfigurations in exposed services.

---

## 7) Lessons learned

- Thorough enumeration is critical to identifying multiple attack vectors.
- Misconfigured file‑sharing services can expose sensitive system data.
- Chaining small vulnerabilities together can lead to full system compromise.
- Vulnerable service modules can allow unexpected file manipulation.
- SUID binaries must be carefully audited to prevent privilege escalation.
- Environment variables such as `PATH` can introduce security risks when used improperly.

---

## 8) Links / Resources

- Room: https://tryhackme.com/room/kenobi
- Nmap documentation
- Exploit‑DB / searchsploit
- Linux privilege escalation techniques