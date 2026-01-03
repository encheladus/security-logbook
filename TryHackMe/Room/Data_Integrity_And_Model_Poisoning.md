# Data Integrity & Model Poisoning — TryHackMe - Web Application Red Teaming

**Date:** 2026-01-03  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Short summary (1–3 lines):

This lab focuses on data integrity risks in LLM systems, specifically supply chain attacks and model poisoning. The objective is to understand how compromised datasets, pre-trained models, or training pipelines can permanently corrupt model behaviour.

---

## 2) Initial hypothesis

Before starting the lab, I expected that:

- Compromised datasets or model components could introduce persistent vulnerabilities.
- Attackers could poison models during training or fine-tuning rather than at inference time.
- Externally sourced models, adapters, and libraries would represent a major attack surface.
- Model poisoning would be difficult to detect using standard benchmark-based evaluation.

---

## 3) Tools used

- Browser-based TryHackMe classroom
- Conceptual analysis of ML and LLM pipelines
- Threat modeling of data ingestion and training workflows

(No sensitive datasets, credentials, or private tooling were used.)

---

## 4) Approach (high level)

- Analyse the LLM supply chain, including datasets, pre-trained weights, adapters, and dependencies.
- Study how poisoning can be introduced during pre-training, fine-tuning, or continual learning.
- Examine real-world examples of supply chain compromises.
- Map attack techniques to OWASP GenAI and LLM threat categories.
- Evaluate defensive strategies from both red team and secure coding perspectives.

---

## 5) Results / Evidence (sanitized)

Observed outcomes included:

- Demonstrations showing that poisoned datasets or model weights can bias or backdoor a model while preserving normal benchmark performance.
- Examples of malicious pre-trained models or adapters propagating unwanted behavior downstream.
- Scenarios where continuous or feedback-based learning pipelines accepted untrusted data with minimal validation.
- Evidence that trigger-based or context-dependent backdoors are difficult to detect once deployed.

Final state (sanitized):  
Model behaviour was persistently altered due to upstream poisoning, without exploiting runtime vulnerabilities or inference-time attacks.

---

## 6) Recommended remediation

- Enforce strong **data and model provenance** across the entire ML lifecycle.
- Apply **cryptographic integrity checks** (hashes, signatures) to datasets, model weights, and adapters.
- Restrict training inputs to **trusted and vetted sources**.
- Isolate and sandbox external models or artifacts before integration.
- Avoid retraining on raw user input, logs, or feedback without strict validation.
- Monitor model behaviour continuously against known-clean baselines.
- Design rollback strategies to recover from poisoned models quickly.

---

## 7) Lessons learned

- Model poisoning is a **persistent and upstream attack**, not a runtime exploit.
- Benchmark accuracy does not guarantee model integrity.
- Supply chain attacks exploit trust assumptions rather than software bugs.
- Continuous learning significantly expands the attack surface.
- Preventing poisoning is far easier than remediating it after deployment.

---

## 8) Links / Resources

- TryHackMe Room: Web Application Red Teaming  
- OWASP GenAI / LLM Top 10 (LLM03)  
- Public write-up (sanitised): Notion / GitHub