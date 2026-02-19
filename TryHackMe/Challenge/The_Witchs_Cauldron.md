# The Witch's Cauldron — TryHackMe

**Date:** 2026-02-18  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

Cryptography challenge focused on securely sharing a secret recipe between two parties over an insecure channel.  
The objective was to understand and practically apply the Diffie-Hellman key exchange, then use the derived shared secret to decrypt protected data.

---

## 2) Initial hypothesis

The scenario clearly suggested a secure communication problem rather than a classical exploitation path.  
My hypothesis was that the lab focused on key exchange mechanisms (likely Diffie-Hellman) instead of direct encryption, meaning the core challenge would be deriving a shared secret rather than breaking encryption itself.

---

## 3) Tools used

- OpenSSL  
- Linux CLI  

(No sensitive parameters, private keys, or credentials included.)

---

## 4) Approach (high level)

- Identify that Diffie-Hellman is used for key exchange.
- Understand the role of public parameters (p, g).
- Differentiate between private keys and public keys.
- Derive the shared secret using one private key and the other party’s public key.
- Use the derived shared secret as a symmetric key.
- Decrypt the encrypted file to retrieve the final flag.

The focus was conceptual: separate key exchange from encryption.

---

## 5) Results / Evidence (sanitized)

- Successfully derived an identical shared secret from both sides.
- Used the shared secret to decrypt the encrypted file.
- Retrieved the final flag from the decrypted content.
- Confirmed that Diffie-Hellman does not encrypt data directly but securely establishes a symmetric key.

Flags obtained:

- THM{y0u_br3w3d_7h3_53cr37}  
- THM{525403e42fbda51dfd0572025d78062f}

---

## 6) Recommended remediation

To securely implement key exchange in real-world systems:

- Use standardized, well-tested cryptographic libraries.
- Prefer modern implementations such as ECDH over classical DH when possible.
- Enforce strong parameter sizes (minimum 2048-bit for classical DH).
- Combine key exchange with authenticated mechanisms to prevent MITM attacks.
- Use derived secrets only through proper key derivation functions (KDF).
- Apply strong symmetric encryption (e.g., AES-256) after key establishment.

---

## 7) Lessons learned

- Diffie-Hellman solves the problem of securely establishing a shared key over an insecure channel.
- It does not perform encryption by itself.
- Private keys are never transmitted.
- The shared secret is mathematically identical on both sides.
- Security relies on the computational hardness of the discrete logarithm problem.
- Clear separation between:
  - Asymmetric cryptography (key exchange)
  - Symmetric cryptography (data encryption)
- Practical usage of OpenSSL for:
  - Generating DH parameters
  - Generating private/public keys
  - Deriving shared secrets
  - Decrypting AES-encrypted data

---

## 8) Links / Resources

- Room: TryHackMe – The Witch's Cauldron  
- OpenSSL Documentation  
- Personal detailed notes (Notion)

---