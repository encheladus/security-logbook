# LLM Output Handling & Privacy Risks — TryHackMe - Web Application Red Teaming

**Date:** 2026-01-02  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Short summary (1–3 lines):

This lab covers security risks introduced by Large Language Model (LLM) outputs, focusing on improper output handling and sensitive information disclosure. The objective is to understand how unsafe assumptions about model outputs can lead to downstream vulnerabilities in real-world systems.

---

## 2) Initial hypothesis

Before starting the lab, I expected that:

- LLM outputs could act as an attack surface similar to untrusted user input.
- Improper handling of generated responses might enable injection-style attacks downstream.
- LLMs could unintentionally disclose sensitive or internal information through over-completion or context leakage.
- These issues could be chained with other system components to escalate impact.

---

## 3) Tools used

- Browser-based lab environment
- Conceptual analysis of LLM pipelines and integrations
- Prompt interaction within the TryHackMe classroom

(No credentials, secrets, or sensitive scripts were used.)

---

## 4) Approach (high level)

- Analyse how LLM-generated output is consumed by applications.
- Identify trust boundaries where model responses cross into execution or rendering contexts.
- Examine examples of downstream usage (frontend rendering, templates, automation).
- Observe how prompt manipulation influences model output.
- Map observed behaviours to OWASP LLM Top 10 categories.

---

## 5) Results / Evidence (sanitized)

Observed outcomes included:

- Model outputs containing HTML, JavaScript, or template syntax that would be dangerous if rendered directly.
- Generated structured content (commands, queries, configuration-like text) that could be misused if executed automatically.
- Responses that revealed internal logic explanations or contextual details beyond what end users should see.
- Demonstrations showing that prompt manipulation alone can lead to sensitive information exposure.

Final state (sanitized):  
LLM output treated as trusted data resulted in unsafe downstream behaviour without exploiting traditional software bugs.

---

## 6) Recommended remediation

- Treat all LLM output as **untrusted data**.
- Apply strict output encoding and escaping before rendering in any frontend context.
- Use context-aware sanitisation based on where the output is consumed (HTML, JSON, templates, shell).
- Prevent direct execution or action triggering from model output.
- Enforce output schemas and allowlists where possible.
- Minimise sensitive data injected into prompts.
- Ensure strong session and context isolation.
- Harden system prompts and avoid embedding operational secrets.

---

## 7) Lessons learned

- LLM output can be as dangerous as unvalidated user input.
- Output-based vulnerabilities are subtle and often emerge across system boundaries.
- Sensitive information disclosure can occur without explicit malicious intent.
- The more autonomy and access an LLM has, the higher the blast radius of a single mistake.
- Most LLM security failures stem from unsafe integration patterns, not the model itself.

---

## 8) Links / Resources

- TryHackMe Room: Web Application Red Teaming  
- OWASP Top 10 for LLM Applications (2025)  
- Public write-up (sanitised): Notion / GitHub

---