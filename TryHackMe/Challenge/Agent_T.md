# Agent T — TryHackMe

**Date:** 2026-02-09  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room focuses on identifying and exploiting a vulnerability affecting a web server.  
The objective is to analyze the exposed service, discover what is “off” about it, and leverage the weakness to gain system access and retrieve the flag.

---

## 2) Initial hypothesis

Given the web‑based context, the initial assumption was that the attack surface would involve:

- A web application vulnerability
- Misconfigured authentication or access control
- Potential header or server misconfiguration disclosure

The plan was to inspect HTTP traffic, identify server technologies, and research known vulnerabilities linked to discovered versions.

---

## 3) Tools used

- Nmap  
- Burp Suite  
- FFUF  
- Web browser  
- Public exploit database (Exploit‑DB)

---

## 4) Approach (high level)

1. **Service reconnaissance**
   - Performed a version scan to identify running services.
   - Discovered a PHP service running via the built‑in CLI development server.

2. **Web enumeration**
   - Accessed the website through a browser.
   - Observed a static page with no functional features or inputs.
   - Reviewed page source for hidden information.

3. **Directory bruteforcing**
   - Attempted to discover hidden files or endpoints.
   - No additional directories or sensitive files were found.

4. **Traffic interception**
   - Intercepted HTTP requests using Burp Suite.
   - Analyzed headers, cookies, and parameters.
   - No exploitable application logic was identified.

5. **Vulnerability research**
   - Pivoted focus back to the PHP dev server version.
   - Searched public exploit databases for known issues.
   - Identified a matching public exploit affecting the service.

6. **Exploitation & post‑exploitation**
   - Executed the exploit against the target.
   - Gained system‑level access.
   - Located and retrieved the flag file.

---

## 5) Results / Evidence (sanitized)

- Successful exploitation of the PHP CLI development server.
- Unauthorized system access obtained.
- Sensitive file discovered on the host.

**Final state:** Flag successfully retrieved from the system.

---

## 6) Recommended remediation

- Never expose PHP CLI development servers to the internet.
- Use hardened production web servers (Apache, Nginx, etc.).
- Keep services updated and patched.
- Restrict access to development environments.
- Implement network segmentation and firewall rules.
- Disable unnecessary services in production.

---

## 7) Lessons learned

- Service reconnaissance can reveal critical vulnerabilities.
- Development servers are not production‑secure.
- Not all web challenges involve application logic flaws.
- Public exploit databases are valuable during pentests.
- When web enumeration fails, pivot back to infrastructure.

---

## 8) Links / Resources

- Room: TryHackMe — Agent T  
- Exploit‑DB (PHP CLI server vulnerability)  
- PHP built‑in server documentation  
- Personal Notion notes / detailed write‑up

---