# Cryptography Concepts — TryHackMe
**Date:** 2026-03-03  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context
This room provides an understanding of cryptography in our everyday digital encounters.  
The goal is to learn **how secrets are protected and tampering detected** in real-world scenarios.

## 2) Initial hypothesis
- Explain what cryptography is and why it matters for confidentiality and integrity.  
- Describe the difference between plaintext and ciphertext with examples.  
- Explain keys and algorithms, and why keeping keys secret is critical.  
- Explain symmetric vs asymmetric encryption using analogies like lockboxes and mailboxes.  
- Describe how symmetric and asymmetric encryption work together to protect web browsing.

## 3) Tools used
- None specific; conceptual understanding and examples (Caesar cipher, analogies).

## 4) Approach (high level)
- Learn the basic concepts of symmetric encryption: plaintext, ciphertext, keys, algorithms.  
- Use analogies (lockbox, mailbox) to understand encryption workflows.  
- Examine classical examples like the Caesar cipher to illustrate algorithm + key.  
- Understand asymmetric encryption and how it solves key distribution issues.  
- Explore real-world applications: HTTPS, certificates, hybrid encryption.  

## 5) Results / Evidence (sanitized)
- Symmetric encryption uses **one secret key** for both encryption and decryption; fast and efficient but requires secure key distribution.  
- Caesar cipher demonstrated algorithm + key: ciphertext unreadable without the key.  
- Asymmetric encryption uses **public/private key pairs**, allowing secure key exchange without prior secret sharing.  
- Hybrid approach (as in HTTPS) combines both: asymmetric key exchange + symmetric session encryption.  
- Digital certificates and Certificate Authorities validate public keys to prevent MITM attacks.  

## 6) Recommended remediation
- Always use proven cryptographic algorithms and libraries.  
- Keep private keys secret; rotate keys periodically.  
- Use hybrid cryptography for secure communications: asymmetric key exchange + symmetric session encryption.  
- Validate certificates to prevent man-in-the-middle attacks.  

## 7) Lessons learned
- Cryptography transforms readable data into unreadable ciphertext to protect information.  
- Security relies on key secrecy, not hiding algorithms.  
- Symmetric encryption is fast but requires secure key distribution.  
- Asymmetric encryption solves key distribution but is slower; used for key exchange and authentication.  
- Modern secure communication uses hybrid cryptography.  
- Understanding key exchange is fundamental for pentesting TLS, certificates, and MITM scenarios.  

## 8) Links / Resources
- [TryHackMe – Cryptography](https://tryhackme.com/room/cryptography)  
- Public write-ups or Notion pages on cryptography concepts  
- Useful docs: Caesar cipher, AES, TLS/HTTPS overviews 