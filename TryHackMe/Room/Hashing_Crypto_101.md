# Hashing - Crypto 101 — TryHackMe - Crypto
**Date:** 2026-01-22  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context
Short summary: An introduction to Hashing as part of a series on cryptography. The lab covers the fundamentals of hashes, password security, and data integrity verification.

Key concepts:

- **Plaintext**: Data before encryption or hashing. Can be text, files, images, etc.  
- **Encoding**: Not encryption. Reversible representation (Base64, hex).  
- **Hash**: Output of a hash function. Irreversible fingerprint of data.  
- **Brute force**: Trying all possible passwords or keys.  
- **Cryptanalysis**: Attacking cryptography by finding mathematical weaknesses.

---

## 2) Initial hypothesis
What’s a hash? What is it used for? Can it be cracked?

---

## 3) Tools used
- hashID  
- Hashcat  
- John the Ripper  
- Linux utilities (sha256sum)  

(*No sensitive data or options included*)

---

## 4) Approach (high level)
1. Understand hash functions and their properties (irreversibility, avalanche effect).  
2. Identify hashes manually and with automated tools.  
3. Test password verification and cracking for both unsalted and salted hashes.  
4. Verify file integrity with hashes.  
5. Explore HMAC usage for message authenticity.

---

## 5) Results / Evidence (sanitized)
- Correctly identified hash types using prefixes, lengths, and encoding.  
- Unsalted hashes cracked using `rockyou.txt` wordlist.  
- Salted hashes required combining each password candidate with salt before hashing.  
- Verified file integrity using SHA256 checksums.  
- Successfully generated HMAC values for secure message authentication.

Challenges encountered:

- Confusion between encoding and hashing.  
- Manual hash identification tricky for unknown formats.  
- Understanding salt protection against rainbow tables.  
- GPU acceleration required for practical cracking; VMs limited.

---

## 6) Recommended remediation
- Never store passwords in plaintext or using fast, unsalted hashes.  
- Use salted, slow hash functions (`bcrypt`, `sha512crypt`) for passwords.  
- Verify file integrity using hashes when appropriate.  
- Use HMACs to ensure both integrity and authenticity for sensitive data.  
- Avoid outdated hash algorithms (MD5, SHA-1) for security purposes.

---

## 7) Lessons learned
- Hash functions are irreversible by design.  
- Salts are essential to prevent rainbow table attacks.  
- Collision resistance is critical; insecure algorithms should be avoided.  
- Hashing serves two main purposes: password verification and integrity checking.  
- GPU acceleration significantly impacts password-cracking speed.  
- Manual hash recognition is an important skill alongside automated tools.  
- HMACs extend hashing for authenticity and integrity.

---

## 8) Links / Resources
- [TryHackMe - Hashing Room](https://tryhackme.com/room/hashing)  
- [Hashcat Example Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)  
- Public write-ups and notes on hashing (Notion or other resources)
