# Data Representation — TryHackMe (Computer Science)

**Date:** 2026-02-27  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room from TryHackMe (Computer Science path) focuses on how computers represent numbers and colors internally.  
The objective was to understand binary logic, positional number systems, and how RGB colors are stored using bits and hexadecimal notation.

---

## 2) Initial hypothesis

Before starting, I expected to understand:

- How 8 colors can be represented using binary  
- How modern systems represent 16 million colors  
- How binary numbers work internally  
- Why hexadecimal is widely used in computing  
- The relationship between binary, decimal, hexadecimal, and octal  

---

## 3) Tools used

- Mental math  
- Binary / hexadecimal conversions  
- No external exploitation tools required  

---

## 4) Approach (high level)

- Reviewed how physical hardware represents two states (0 and 1)  
- Understood how multiple bits increase possible combinations using 2ⁿ  
- Studied RGB color representation with 3-bit and 24-bit models  
- Practiced converting between:
  - Binary → Decimal  
  - Binary → Hexadecimal  
  - Hexadecimal → Decimal  
  - Octal → Decimal  
- Identified the positional rule shared by all number systems  

---

## 5) Results / Evidence (sanitized)

Key validated concepts:

- 1 bit = 2 possible states  
- n bits = 2ⁿ possible values  
- 8 bits = 1 byte = 256 values  
- RGB (modern systems) = 24 bits total  
- 256 × 256 × 256 = 16,777,216 possible colors  

Color representation findings:

- 3 bits (1 per RGB channel) → 8 total colors  
- 24 bits (8 per channel) → 16,777,216 colors  
- 4 bits = 1 hexadecimal digit  
- 1 byte = 2 hexadecimal digits  

Example:

Binary:
```
10100011 11101010 00101010
```

Grouped into 4 bits:
```
1010 0011 1110 1010 0010 1010
```

Hexadecimal:
```
A3EA2A
```

Each pair corresponds to one RGB channel:
- A3 → Red  
- EA → Green  
- 2A → Blue  

---

## 6) Recommended remediation

Although this room is theoretical, strong data representation fundamentals are essential in secure development:

- Use strict type validation when handling numeric input  
- Be aware of encoding formats when processing user data  
- Validate length and format of hexadecimal values  
- Avoid implicit type conversions in backend logic  
- Enforce proper bounds checking when handling binary data  

---

## 7) Lessons learned

- Everything in computing reduces to binary (0 and 1).  
- The number of possible values grows exponentially (2ⁿ).  
- All number systems follow the same positional power rule.  
- Hexadecimal is simply a compact and structured representation of binary.  
- RGB colors are just structured binary values grouped into bytes.  
- Mastering these fundamentals will simplify later topics like memory analysis, networking, reverse engineering, and exploit development.  

---

## 8) Links / Resources

- Room: TryHackMe – Computer Science (Data Representation)  
- Useful docs: Binary & Hexadecimal conversion references  
- Notion / detailed write-up (public)  

---