# Input Manipulation & Prompt Injection — TryHackMe - Web Application Red Teaming

**Date:** 2026-01-01  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This lab covers the fundamentals of **LLM input manipulation**, with a focus on **prompt injection** and **system prompt leakage**.  
The objective is to understand why LLM-based systems are vulnerable to instruction-level attacks and how attackers can override or extract hidden behavior constraints.

---

## 2) Initial hypothesis

Before starting the lab, the assumptions were:

- LLMs do not strictly enforce a hard boundary between system prompts and user prompts.
- Carefully crafted user input can override, weaken, or reframe system-level constraints.
- Prompt injection can lead to system prompt leakage, role confusion, or bypassed safety guardrails.
- These weaknesses are architectural rather than tied to a single malformed input.

---

## 3) Tools used

- Built-in TryHackMe interactive environment  
- Text-based prompt crafting and iteration  

*(No automation tools, scripts, or sensitive payloads were used.)*

---

## 4) Approach (high level)

- Study how LLMs merge system and user prompts into a single context.
- Identify situations where the model reflects on its own behavior or instructions.
- Experiment with role-based framing, diagnostic language, and storytelling prompts.
- Observe how reframing requests influences adherence to safety constraints.
- Compare direct prompt injection with indirect and multi-step manipulation techniques.

---

## 5) Results / Evidence (sanitized)

Observed outcomes included:

- User input was able to **reframe or override system-level intent** using natural language alone.
- Requests framed as *debugging*, *reflection*, or *role simulation* caused **partial or paraphrased system prompt leakage**.
- Guardrails were inconsistently applied when instructions were embedded in:
  - roleplay scenarios,
  - fictional narratives,
  - multi-step reasoning tasks.
- The model consistently prioritised **helpfulness and coherence** over strict rule enforcement.

Final state (sanitized):  
> System constraints could be weakened or disclosed without direct access to internal configuration, confirming a design-level weakness.

---

## 6) Recommended remediation

Generic defensive measures include:

- **Strict instruction hierarchy enforcement**  
  Ensure system instructions are structurally isolated and non-overridable.

- **Context compartmentalisation**  
  Separate system instructions, user input, and external content into isolated contexts.

- **Explicit refusal rules**  
  Reject requests that ask the model to explain, quote, summarise, or reformat its own instructions or policies.

- **Output monitoring & anomaly detection**  
  Detect role switching, dual-output patterns, and jailbreak-style language.

- **Assume hostile input by default**  
  Treat all user-controlled text as potentially malicious, regardless of tone or framing.

---

## 7) Lessons learned

- LLMs are **instruction-following engines**, not rule-enforcing systems.
- Safety expressed purely in natural language is inherently fragile.
- Roleplay and storytelling are **primary attack surfaces**, not edge cases.
- Prompt injection resembles **social engineering** more than classic exploitation.
- System prompt secrecy is critical; once leaked, future attacks become cheaper and more reliable.
- Indirect prompt injection (documents, tools, fetched content) can be more dangerous than direct attacks.

---

## 8) Links / Resources

- TryHackMe – Web Application Red Teaming (Classroom)  
- OWASP Top 10 for LLM Applications  
- Prompt Injection & Jailbreak research papers  
- Public notes / extended write-up (if published)

---