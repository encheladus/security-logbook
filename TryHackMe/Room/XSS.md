# Cross-Site Scripting (XSS) — TryHackMe / Web Application Pentesting  
**Date:** 2025-11-03  
**Type:** TryHackMe / Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context
Short summary (1–3 lines): Hands-on study of Cross-Site Scripting (XSS) fundamentals on a TryHackMe Web App Pentesting classroom. Covers reflected, stored and DOM XSS types, browser/JS nuances, and context-aware defenses so you can reliably map inputs → sinks and prove low-impact PoCs.

## 2) Initial hypothesis
What you expected before starting:  
If untrusted input is output to a web page without context-aware escaping/sanitization (or unsafe DOM APIs are used), an attacker can execute arbitrary JavaScript in other users’ browsers (XSS), enabling session theft, UI manipulation, and data exfiltration.

## 3) Tools used
Short list:  
- Burp Suite (Proxy, Repeater)  
- Browser DevTools (Console, Network, DOM inspection)  
- curl / HTTP client for quick requests  
- Text editor / notes (for mapping inputs → sinks)  

*(Do not include options, credentials, or sensitive scripts.)*

## 4) Approach (high level)
- Map all possible inputs (query params, form fields, hash, storage, headers) and identify where each value is rendered.  
- Classify each sink by context: HTML body, attribute, JavaScript string, or DOM API.  
- Craft minimal, non-destructive PoCs (start with `alert(1)`) per context to verify execution.  
- Test reflected and stored flows server-side; inspect client JS for DOM flows (look for `location.*`, `innerHTML`, `insertAdjacentHTML`, `eval`, etc.).  
- Iterate encodings/obfuscations only to confirm contexts and decoding steps — avoid destructive payloads.

## 5) Results / Evidence (sanitized)
What I observed:  
- No single exploitable production target disclosed here; lab exercise confirms that XSS is fundamentally a **context** problem.  
- Verified that minimal PoCs (e.g., `alert(1)`) execute when unescaped input reaches a vulnerable sink in the right context.  
- Final state (sanitized evidence): mapped inputs → sinks for page(s) tested and reproduced safe PoCs showing execution in the browser console or alert dialog.  
  - Example proof token: `PoC: alert(1) executed in <context type>` (non-sensitive, non-destructive).  
- Note: DOM XSS paths left no server traces — confirmed by successful client-side payloads that never hit the server logs.

## 6) Recommended remediation
Generic technical measures:  
- Apply **context-aware output encoding** on every render (HTML body, attribute, JS string, URL).  
- Avoid dangerous sinks: prefer textContent / setAttribute / createTextNode over `innerHTML` and `document.write`.  
- Use vetted sanitizers when HTML must be allowed (whitelist approach).  
- Enforce strict Content Security Policy (CSP) without `unsafe-inline` and with explicit `script-src` / `base-uri` / `object-src` rules.  
- Enforce input validation (as defense in depth) and store canonicalized values.  
- Use HTTPOnly and Secure cookie flags; rotate/session-bind tokens to reduce impact of stolen cookies.  
- Static analysis / SAST for common DOM sinks and CI checks for templating misuses.  
- Review and sandbox third-party widgets; restrict their privileges and isolate with iframes where feasible.

## 7) Lessons learned
- XSS is first and foremost a **context** problem: identical input is safe or dangerous depending on where it is placed (body vs attribute vs JS vs DOM).  
- Start with minimal, non-destructive PoCs (`alert(1)`) then expand only if scope/authorization allows.  
- DOM XSS can leave zero server evidence — always review client JS sources and sinks.  
- Proper defenses must be applied at render time (output encoding) and in code (safe DOM APIs, sanitizers).  
- Classify quickly: reflected (link), stored (persistent), DOM (client-only) — each requires different testing and remediation focus.

---

## 8) Links / Resources
- Room / machine: TryHackMe — *Web Application Pentesting (classroom)*  
- Useful docs: OWASP XSS Prevention Cheat Sheet; MDN docs on DOM APIs and CSP.  
- Notion / detailed write-up (public): (link to your public write-up)

---