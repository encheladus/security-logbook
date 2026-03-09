# The CIA Triad — TryHackMe

**Date:** 2026-03-02  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room introduces the CIA Triad and explains how it forms the foundation of cyber security. The objective is to understand how Confidentiality, Integrity, and Availability define what cyber security actually protects in the digital world.

---

## 2) Initial hypothesis

Before starting, I expected to:

- Understand the three core pillars of cyber security  
- Learn the purpose of Confidentiality, Integrity, and Availability  
- Recognize these principles in simple real-world and digital scenarios  
- Develop a mindset focused on preserving these security aspects  

---

## 3) Tools used

- TryHackMe classroom environment  
- Personal note-taking  

---

## 4) Approach (high level)

I began by understanding how information storage evolved from physical documents to digital systems. Unlike paper-based information, digital data is constantly transmitted, copied, and stored across networks and cloud infrastructures, which increases exposure.

I then studied the three principles of the CIA Triad and analyzed them through:

- Real-world analogies  
- Practical digital examples  
- Security failures and their consequences  

For each principle, I focused on:
- What it protects  
- How it can be compromised  
- Typical defensive mechanisms used to preserve it  

---

## 5) Results / Evidence (sanitized)

I understood that cyber security fundamentally protects three aspects of digital information:

- **Confidentiality**: Ensures sensitive data is accessible only to authorized individuals.  
  Example impact: credential theft through insecure public Wi-Fi leading to unauthorized access.

- **Integrity**: Ensures data remains accurate and unaltered.  
  Example impact: modifying a bank transaction recipient account, redirecting funds.

- **Availability**: Ensures systems and services are accessible when needed.  
  Example impact: a website becoming unreachable due to excessive traffic (Denial of Service).

I realized that every attack targets at least one of these principles, and every defensive control aims to protect one or more of them.

---

## 6) Recommended remediation

To preserve the CIA principles, organizations should implement:

- Encryption to protect confidentiality  
- Strong access control mechanisms  
- Integrity verification mechanisms (validation, approval workflows, change controls)  
- Redundancy systems and backups to ensure availability  
- Traffic management and resilience mechanisms to prevent service disruption  

Security controls should always be mapped to the pillar they are protecting.

---

## 7) Lessons learned

- Cyber security is not just about tools but about protecting data conditions.  
- Every vulnerability can be categorized based on its impact on Confidentiality, Integrity, or Availability.  
- Thinking in terms of impact is essential for penetration testing and bug bounty reporting.  
- A valid vulnerability report must clearly explain which pillar is affected.  
- The CIA Triad is a decision-making framework for risk analysis and system design.  

---

## 8) Links / Resources

- Room: TryHackMe – Cyber Security Intro  
- Personal notes: Notion (detailed version)  

---