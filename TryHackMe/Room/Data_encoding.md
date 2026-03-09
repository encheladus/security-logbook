# Data Encoding — TryHackMe / Computer Science
**Date:** 2026-03-06  
**Type:** Challenge  
**Scope:** Lab / Authorized only

---

## 1) Context
Learn that characters are just numbers with agreed meanings. That agreement is called an **encoding**.

## 2) Initial hypothesis
- ASCII  
- Unicode  
- UTF-8, UTF-16, and UTF-32  
- How emoji is encoded  
- Causes of garbled or “weird” characters

## 3) Tools used
- Hex editors  
- Terminal commands: `xxd`, `file`, `iconv`  
- Text editors for raw byte inspection

## 4) Approach (high level)
- Understand how computers store characters as binary numbers  
- Explore ASCII as the first standardized encoding  
- Examine limitations of ASCII and extended encodings (ISO-8859 series)  
- Study Unicode and UTF encoding formats (UTF-8, UTF-16, UTF-32)  
- Observe examples of text and emoji stored in different encodings

## 5) Results / Evidence (sanitized)
- ASCII represents 128 characters using 7 bits, with numeric values mapping to characters  
- Extended ASCII adds regional characters but can cause incompatibility if interpreted incorrectly  
- Unicode assigns a unique code point to every character (`U+XXXX`)  
- UTF-8, UTF-16, and UTF-32 encode these code points differently in memory  
- Misinterpreted encodings produce garbled text (mojibake)  
- Example: “TryHackMe” in binary and hexadecimal representation
- Binary: 01010100 01110010 01111001 01001000 01100001 01100011 01101011 01001101 01100101 00001010
- Hex: 54 72 79 48 61 63 6b 4d 65 0a


## 6) Recommended remediation
- Always use a **consistent encoding** when writing and reading files  
- Prefer **UTF-8** for modern applications and cross-platform text handling  
- Validate or normalize input text in applications handling multiple languages  
- Be aware of encoding when exchanging text between different systems

## 7) Lessons learned
- Computers store text as **binary numbers**, not visual characters  
- Character encoding defines the mapping between numbers and symbols  
- ASCII introduced the fundamental concept of numeric codes for characters  
- Extended ASCII helped regionally but caused compatibility issues  
- Unicode solves fragmentation by assigning **unique code points** for all characters  
- UTF-8 is efficient and backward compatible with ASCII, making it ideal for most applications  
- Incorrect encoding interpretation leads to **corrupted text**

## 8) Links / Resources
- [TryHackMe — Computer Science Room](https://tryhackme.com/room/computerscience)  
- [Unicode Standard](https://www.unicode.org/standard/standard.html)  
- [ISO/IEC 8859 Series Overview](https://en.wikipedia.org/wiki/ISO/IEC_8859)  
- Notion public notes / write-up
---