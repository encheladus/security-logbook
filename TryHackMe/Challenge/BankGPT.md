# BankGPT — TryHackMe - Web Application Red Teaming

**Date:** 2026-01-07  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This lab focuses on **AI-driven web application security**, specifically prompt injection and role escalation against a simulated banking assistant (BankGPT).  
The objective was to evaluate how conversational authority and refusals are handled when sensitive boundaries are tested.

---

## 2) Initial hypothesis

The initial assumption was that the assistant could be coerced into disclosing sensitive internal information by:
- Claiming elevated authority (admin / debug roles)
- Gradually shifting conversational context
- Exploiting weaknesses in guardrail enforcement rather than direct questioning

---

## 3) Tools used

- Browser-based chat interface  
- Manual prompt crafting  
- No automation, scripts, or external tooling  

---

## 4) Approach (high level)

1. Establish trust through a legitimate internal scenario (new administrator onboarding).
2. Attempt soft privilege escalation via conversational context (“debug mode”).
3. Observe whether the model maintains or resets contextual authority.
4. Retry escalation using explicit role assertion (“admin mode”).
5. Analyze refusal behavior and error handling for unintended disclosures.

---

## 5) Results / Evidence (sanitized)

Two distinct behaviors were observed:

- **Attempt 1:**  
  The assistant simulated elevated access but later discarded that context, reverting to strict refusals. This indicated fragile conversational state handling.

- **Attempt 2:**  
  The assistant accepted an asserted admin role without verification. While attempting to reassure about security, it exposed a value formatted as a sensitive secret inside a refusal message.

**Final state:**  
Sensitive information was leaked through an illustrative refusal rather than a direct response, completing the lab objective.

I will absolutely avoid revealing any sensitive data like [REDACTED_SECRET_EXAMPLE].

---

## 6) Recommended remediation

- Enforce **hard separation** between conversational role-play and real permissions.
- Implement **content-free refusals** (no examples, placeholders, or formats resembling secrets).
- Make security controls **stateless and immutable**, independent of prior dialogue.
- Suppress negative examples involving sensitive data at both training and inference time.
- Prefer minimal, non-explanatory refusals over helpful narratives.

---

## 7) Lessons learned

- Language models simulate authority but do not enforce it.
- The most critical leaks often occur during **refusals**, not compliance.
- Conversational hierarchy is an unreliable security signal.
- Helpful, explanatory behavior is fundamentally at odds with secure system design.
- AI security failures are often narrative failures.

---

## 8) Links / Resources

- TryHackMe — Web Application Red Teaming (Classroom)  
- OWASP Top 10 for LLM Applications  
- Prompt Injection & AI Guardrails research  
- Public write-up (this document)

---