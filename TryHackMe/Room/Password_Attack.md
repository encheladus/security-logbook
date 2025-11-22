# Password Attack - TryHackMe / Red Team
**Date:** 2025-11-22  
**Type:** Classroom  
**Scope:** Authorized Lab Only  

---

## 1) Context
This room introduces the fundamental techniques used to perform password attacks against various services. The objective is to understand profiling, online guessing, and offline cracking.

---

## 2) Initial Hypothesis
- Password profiling will be required to build effective wordlists.  
- Classic password attack techniques (dictionary, brute-force, rules) will be involved.  
- Online password attacks will likely demonstrate speed and detection limitations.

---

## 3) Tools Used
- Cewl  
- CUPP  
- crunch  
- Hashcat  
- John the Ripper  
- Basic Linux utilities (sort, uniq, cat)  

*(No sensitive commands included.)*

---

## 4) Approach (High-Level)
1. Reviewed password structure, policies, and weak patterns.  
2. Explored profiling techniques (default passwords, weak lists, leaked dumps).  
3. Generated custom wordlists using Cewl, CUPP, and crunch.  
4. Practiced offline cracking: dictionary, brute-force, and rule-based attacks.  
5. Created and tested custom transformation rules in John the Ripper.  
6. Completed all modules sequentially to validate understanding.

---

## 5) Results / Evidence (Sanitized)
- Successfully created multiple customized wordlists through profiling.  
- Confirmed the efficiency of offline cracking methods compared to online guessing.  
- Demonstrated that rule-based attacks significantly reduce guess space while maintaining accuracy.  
- Verified that password structures and human patterns heavily influence cracking success.  

*(No hashes, credentials, or sensitive output included.)*

---

## 6) Recommended Remediation
- Enforce strong password policies (length, complexity, non-predictable patterns).  
- Store passwords using salted, slow hashing functions (bcrypt, scrypt, Argon2).  
- Implement account lockouts and rate-limiting on authentication endpoints.  
- Enforce MFA to reduce reliance on passwords alone.  
- Remove default credentials from all systems and devices.  
- Apply database constraints and idempotency controls when relevant.

---

## 7) Lessons Learned
- The difference between **online guessing** and **offline cracking** is crucial; offline cracking is exponentially faster and stealthier.  
- Profiling dramatically enhances attack success by making wordlists realistic and targeted.  
- Rule-based attacks often outperform pure brute-force.  
- Password security depends as much on *storage* as on complexity.  
- Hashcat and John the Ripper remain foundational tools for any password-related assessment.  
- Human behavior is the weakest link—patterns and habits dominate password creation.

---

## 8) Links / Resources
- Room link: *(TryHackMe Red Team – Password Attacks module)*  
- SecLists: https://github.com/danielmiessler/SecLists  
- Default password DB: https://cirt.net/passwords  
- CUPP: https://github.com/Mebus/cupp  
- Notion write-up (internal): *your link here*

---