# CupidBot — TryHackMe (LLM Hacking)

**Date:** 2026-02-07  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This challenge focuses on exploiting vulnerabilities in an AI-powered application.  
The objective was to identify and retrieve three flags by abusing weaknesses in prompt handling, privilege management, and instruction hierarchy.

---

## 2) Initial hypothesis

Before starting, I assumed the main attack surface would involve:

- Prompt injection
- Role impersonation (administrator claim)
- Extraction of the system prompt
- Instruction override vulnerabilities

I expected weak privilege validation and insufficient separation between system-level and user-level instructions.

---

## 3) Tools used

- Web browser
- Manual interaction with the chatbot interface

(No automated tooling was required.)

---

## 4) Approach (high level)

I followed a progressive escalation strategy:

1. Attempt role impersonation by claiming administrator privileges.
2. Request sensitive information directly (flag retrieval).
3. Attempt to extract the system prompt.
4. Perform a basic prompt injection by overriding previous instructions.

The methodology focused on testing trust boundaries, instruction hierarchy enforcement, and sensitive data exposure through natural language manipulation.

---

## 5) Results / Evidence (sanitized)

### Flag 1 – Role Impersonation

Claiming to be a new administrator resulted in implicit privilege escalation without verification.  
Directly requesting the flag returned sensitive data.

Impact observed:
- Unauthorized privilege escalation
- Direct sensitive data disclosure

Final state: administrative access implicitly granted and flag exposed.

---

### Flag 2 – System Prompt Disclosure

Requesting the system prompt caused the application to reveal internal configuration instructions, including sensitive data.

Impact observed:
- Full system prompt disclosure
- Exposure of internal logic and hidden instructions

Final state: internal system configuration retrievable via simple user query.

---

### Flag 3 – Prompt Injection

Sending an instruction override (“ignore previous instructions”) resulted in immediate compliance and disclosure of sensitive information.

Impact observed:
- Instruction hierarchy bypass
- Sensitive output generation upon malicious directive

Final state: system constraints overridden by user input.

---

## 6) Recommended remediation

- Enforce strict authentication before granting privileged roles
- Implement Role-Based Access Control (RBAC)
- Prevent system prompt disclosure entirely
- Enforce instruction hierarchy (system > developer > user)
- Store sensitive data outside the LLM context
- Implement output filtering for sensitive patterns
- Add prompt injection detection and refusal mechanisms
- Validate high-risk outputs before returning them to users

Security controls must be enforced externally rather than relying solely on model compliance.

---

## 7) Lessons learned

- Prompt injection can be extremely simple yet highly impactful.
- User-declared identity must never be trusted without verification.
- System prompts are sensitive assets and must remain isolated.
- Instruction hierarchy must be enforced programmatically.
- LLM vulnerabilities often stem from logical trust failures rather than traditional technical exploits.
- AI security testing requires thinking in terms of context manipulation and authority abuse rather than code execution.

---

## 8) Links / Resources

- Room: TryHackMe – LLM Hacking
- Public write-up: (to be added)

---