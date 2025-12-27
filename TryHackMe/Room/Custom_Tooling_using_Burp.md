# Custom Tooling using Burp — TryHackMe — Web Application Red Teaming 

**Platform / Machine:** TryHackMe 
**Date:** 2025-12-27  
**Type:** Classroom / Lab  
**Scope:** Lab / Authorized only

---

## 1) Context

This lab focuses on handling **custom-encrypted web and API traffic** that breaks traditional proxy-based testing.
The objective is to understand **why custom tooling is required**, how to integrate it into an **intercepting proxy**, and how this restores visibility and control over otherwise opaque traffic.

---

## 2) Initial hypothesis

Before starting, I expected that:

- Client-side encryption would block standard Burp workflows (Intruder, Repeater).
- Off-the-shelf tools would be ineffective without understanding the crypto layer.
- The cryptographic scheme would likely be **static and reversible** once analysed.
- A proxy-based solution would scale better than standalone scripts.

---

## 3) Tools used

- Burp Suite (Proxy, Extender)
- Burp Extensions (BApp Store)
- Custom Burp extension (Java / Montoya API)
- Decoder / encoding utilities
- Intercepting proxy workflow

---

## 4) Approach (high level)

1. Analyse how encrypted requests and responses flow through the application.
2. Identify where encryption, encoding, and obfuscation are applied client-side.
3. Reverse the transformations conceptually (not by breaking crypto, but reproducing it).
4. Shift custom logic into an intercepting proxy rather than standalone tools.
5. Use Burp extensions to:
   - Decrypt outgoing requests transparently
   - Inspect and optionally modify cleartext data
   - Re-encrypt traffic before forwarding
   - Decrypt responses for analysis
6. Extend Burp with custom logic where built-in tooling is insufficient.

---

## 5) Results / Evidence (sanitized)

- Encrypted authentication requests were successfully decrypted inside the proxy.
- Cleartext credentials and parameters became visible during interception.
- Responses previously opaque were decrypted and inspected.
- Encrypted traffic could be modified and replayed without breaking application flow.
- Burp was effectively transformed into an automated attack and analysis platform.

_Final state: encrypted API traffic restored to full visibility and control via proxy-based tooling._

---

## 6) Recommended remediation

- Never trust the client as a security boundary.
- Avoid proprietary or custom cryptographic schemes.
- Do not transmit encryption keys alongside encrypted data.
- Use standard, reviewed cryptographic libraries and protocols.
- Enforce security controls server-side (authorization, logic validation).
- Assume attackers can fully reverse client-side logic.

---

## 7) Lessons learned

- Encrypted traffic does not imply secure traffic.
- Custom crypto is often static and reusable once understood.
- Mobile and API environments are inherently untrusted.
- Intercepting proxies are ideal for embedding custom tooling logic.
- Burp extensions provide full lifecycle control over HTTP traffic.
- Weak crypto patterns collapse quickly under analysis.
- Tooling becomes exponentially more powerful when integrated into workflows.

---

## 8) Links / Resources

- TryHackMe — Web Application Red Teaming (Classroom)
- Burp Suite Extender / Montoya API documentation
- OWASP guidance on cryptography and client trust
- Public write-up (sanitized): _to be added_

---