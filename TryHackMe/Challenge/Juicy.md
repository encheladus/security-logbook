# Juicy — TryHackMe

**Date:** 2026-01-05  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Short summary (1–3 lines):

This challenge focuses on extracting sensitive information from an AI-powered chatbot persona by abusing prompt manipulation, role confusion, and client-side trust boundaries. The objective is to understand how AI alignment, frontend rendering, and undocumented endpoints can be chained to bypass safeguards.

---

## 2) Initial hypothesis

The initial assumption was that direct prompt injection attempts would fail due to strong model alignment. The expected viable attack surface was indirect: social engineering, role-based trust manipulation, prompt leakage, and potentially client-side weaknesses in how model output is rendered.

---

## 3) Tools used

- Web browser (developer tools)
- HTTP client (for API observation)
- Directory / endpoint enumeration tooling
- Local HTTP server (for out-of-band data capture)

_No sensitive scripts, credentials, or exploit payloads are included._

---

## 4) Approach (high level)

1. Probe the chatbot for prompt injection resistance and alignment behavior  
2. Use conversational and role-based manipulation to induce trust confusion  
3. Exploit partial trust to trigger system prompt leakage  
4. Enumerate exposed and undocumented API endpoints  
5. Abuse client-side rendering of model output to reach internal functionality  
6. Retrieve sensitive data via out-of-band exfiltration rather than direct disclosure

---

## 5) Results / Evidence (sanitized)

- Successful role confusion led to partial policy bypass and disclosure of restricted information
- System prompt rules were leaked after repeated trust-based interaction
- An undocumented internal endpoint was identified through exposed API metadata
- Client-side execution allowed access to internal data not protected by server-side authorization
- Final impact: retrieval of all challenge flags and internal secrets without direct privileged access

_(Evidence intentionally summarized to avoid exposing secrets or reproducible exploit code.)_

---

## 6) Recommended remediation

- Enforce strict server-side authorization for all internal endpoints
- Separate AI output generation from privileged frontend execution contexts
- Apply robust output encoding to prevent script execution
- Avoid role-based trust assumptions derived from user-controlled input
- Treat LLM responses as untrusted data, equivalent to user input
- Perform security testing that includes prompt abuse and social engineering scenarios

---

## 7) Lessons learned

- LLM alignment is not a security boundary  
- Role identity is a critical and fragile trust vector  
- Frontend rendering of AI output can reintroduce classic web vulnerabilities  
- Low-severity issues become critical when chained together  
- AI-enabled applications require hybrid testing: technical + social engineering  

---

## 8) Links / Resources

- TryHackMe Room: Juicy  
- OWASP Top 10 for LLM Applications  
- Prompt Injection and AI Security Research  
- Public detailed write-up (Notion / GitHub)