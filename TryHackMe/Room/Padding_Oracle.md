# Padding Oracle — TryHackMe - Web Application Red Teaming

**Date:** 2025-12-22  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

This lab focuses on understanding how **padding works in block cipher encryption** and how improper error handling enables **Padding Oracle Attacks**.  
The objective is to learn how AES-CBC with PKCS#7 padding can be broken at the application level when padding validation leaks information.

---

## 2) Initial hypothesis

Before starting the lab, the following assumptions and focus areas were identified:

- How padding schemes (PKCS#7) operate internally  
- How block cipher modes differ, especially CBC  
- Where padding validation occurs during decryption  
- Whether application responses leak padding validity  
- If the decryption process can be automated  
- What mitigations are required to prevent this class of attack  

---

## 3) Tools used

- Conceptual padding oracle logic  
- HTTP request replaying (lab-provided interface)  
- Automated padding oracle tooling (PadBuster)  

> No credentials, secrets, or sensitive scripts were used or stored.

---

## 4) Approach (high level)

- Review how PKCS#7 padding is applied and validated  
- Study AES block cipher modes and identify CBC characteristics  
- Analyze CBC decryption flow and padding validation order  
- Observe application behavior when ciphertext is modified  
- Identify distinguishable responses indicating valid vs invalid padding  
- Apply byte-by-byte padding oracle logic from the last block backward  
- Use automation to reliably recover intermediate values and plaintext  
- Validate findings by reconstructing the original message  

---

## 5) Results / Evidence (sanitized)

- Modified ciphertexts triggered **different server behaviors** depending on padding validity  
- Valid padding responses acted as a **decryption oracle**  
- Plaintext was recovered **byte by byte without knowing the key**  
- Intermediate decryption values (`Dk(Ci)`) were inferred and XORed to obtain plaintext  
- Final state confirmed full plaintext recovery with correct padding removal  

_No secrets, keys, or raw ciphertexts are disclosed._

---

## 6) Recommended remediation

- Replace AES-CBC with **authenticated encryption (AEAD)**:
  - AES-GCM
  - AES-CCM
- Verify ciphertext **authenticity before decryption**
- Enforce **uniform error handling**:
  - Same HTTP status codes
  - Same error messages
  - Same response timing
- Never expose padding, format, or cryptographic error details
- Use modern, maintained cryptographic libraries with safe defaults

---

## 7) Lessons learned

- Padding is not insecure by design — **observable validation is**
- AES-CBC is mathematically sound but **operationally fragile**
- Padding oracle attacks:
  - Do not require keys
  - Do not break AES
  - Work remotely over HTTP
- The attack recovers **intermediate values first**, then plaintext via XOR
- Most real-world crypto failures come from:
  - Architecture
  - Error handling
  - Application logic  
  not from broken algorithms

---

## 8) Links / Resources

- TryHackMe — Web Application Red Teaming (Classroom)  
- PKCS#7 Padding Specification  
- AES Block Cipher Modes (CBC, CTR, ECB)  
- Padding Oracle Attacks (Vaudenay)  
- PadBuster documentation  

---