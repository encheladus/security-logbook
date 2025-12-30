# WAF: Introduction — TryHackMe - Web Application Red Teaming
**Date:** 2025-12-30  
**Type:** Lab  
**Scope:** Lab / Authorized only

---

## 1) Context
Short summary (1–3 lines):  
This lab introduces Web Application Firewalls (WAFs), explaining their role in web security, how they differ from other firewalls, and how they detect and mitigate application-layer attacks. The learning objective is to understand WAF types, rules, detection methods, and limitations.

---

## 2) Initial hypothesis
Before starting, I expected to learn:  
- How WAFs detect attacks like SQLi, XSS, and LFI.  
- What differentiates WAFs from stateful or next-generation firewalls.  
- Techniques for fingerprinting and bypassing WAFs in a controlled lab environment.

---

## 3) Tools used
- curl  
- wafw00f  
- Nmap (http-waf-fingerprint)  

---

## 4) Approach (high level)
1. Reviewed the evolution of firewalls from stateless to next-generation.  
2. Analysed WAF deployment models: cloud, appliance, host-based.  
3. Studied WAF detection strategies: signature-based, behavioural, hybrid.  
4. Examined OWASP CRS example rules and how input normalisation and scoring work.  
5. Investigated WAF limitations: encrypted traffic, business logic, client-side attacks.

---

## 5) Results / Evidence (sanitized)
- Learned that WAFs:
  - Detect common attacks like SQLi and XSS.  
  - Use signature and behavioural detection, sometimes combined.  
  - Rely on rule sets and anomaly scoring for decisions.  
- Observed that:
  - Passive and active fingerprinting can identify WAF type.  
  - Virtual patching mitigates, but does not fix, vulnerabilities.  
  - WAFs cannot detect broken business logic or client-side attacks.  

---

## 6) Recommended remediation
- Treat WAFs as a defense layer, not a primary fix.  
- Implement secure coding practices and access controls.  
- Use WAFs to complement anomaly monitoring and compliance.  
- Keep rules and signatures updated; tune for false positives.  
- Monitor encrypted traffic handling carefully if TLS termination occurs at the WAF.

---

## 7) Lessons learned
- WAFs are sophisticated but not infallible; their effectiveness depends on configuration, rules, and integration.  
- Identifying a WAF early improves testing efficiency and attack strategy.  
- Signature-based detection is fast but blind to novel payloads.  
- Behavioural detection adapts to traffic patterns but can be computationally expensive.  
- Effective WAF rules combine detection engines, input normalisation, logging, tagging, and scoring.  
- WAFs cannot replace secure application design—defense-in-depth is crucial.

---

## 8) Links / Resources
- [TryHackMe WAF Lab](https://tryhackme.com/room/waf)  
- [OWASP Core Rule Set](https://coreruleset.org/)  
- [wafw00f Documentation](https://github.com/EnableSecurity/wafw00f)  
- [Nmap http-waf-fingerprint script](https://nmap.org/nsedoc/scripts/http-waf-fingerprint.html)  
- Public Notion write-up: [Notion WAF Notes](https://www.notion.so/)
