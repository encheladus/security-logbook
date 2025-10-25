# NoSQL Injection — TryHackMe - Web Application Pentesting  
**Date:** 2025-10-24  
**Type:** TryHackMe / Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context  
**Short summary (1–3 lines):** Walkthrough covering basic NoSQL injection techniques against MongoDB. Goal: understand operator and syntax injection vectors, how they differ from classic SQLi, and practical impact (auth bypass, impersonation, blind data extraction).

## 2) Initial hypothesis  
I expected "NoSQL = safe from SQLi" because there are no SQL strings or obvious quote-escaping vectors. My hypothesis to test: even without SQL syntax, an attacker can tamper with how the app builds MongoDB queries to (1) bypass login, (2) impersonate other users, and (3) extract data.

## 3) Tools used  
Short list: Burp Suite (proxy/HTTP capture), browser devtools, curl (only for conceptual testing).  
— **do not** include options, credentials, or sensitive scripts.

## 4) Approach (high level)  
- Inspect login workflow and capture requests.  
- Attempt to submit specially-shaped parameters that parse as objects/arrays (e.g., `field[$op]=value`) to see how the server builds the query.  
- Test operator-injection vectors (`$ne`, `$nin`, `$regex`, etc.) to alter filter logic (auth bypass, selecting different users).  
- Use behavioral differences (redirects, error vs success responses) to confirm injections and carry out blind extraction with regex-based boolean probes.  
- Avoid running precise commands in the write-up; keep methodology conceptual.

## 5) Results / Evidence (sanitized)  
- **Auth bypass (operator injection):** Supplying array-like parameters that include Mongo operators changed the filter semantics. Example concept: `user[$ne]=admin & pass[$ne]=anything` becomes `{ user: { $ne: "admin" }, pass: { $ne: "anything" } }` and led to a successful login flow when the app used the parsed object directly.  
- **Impersonation / account selection:** Using `$nin` / other selection operators allowed returning a different account than intended (the app returned a user document matching the altered filter).  
- **Blind extraction:** `$regex` in the password field can be used to ask yes/no questions (e.g., `^.{7}$` to test length, `^a.*` to test first char). Observing response differences allowed incremental recovery logic (boolean-based blind extraction).  
- **Sanitized proof:** final state observed — authentication bypass and different account returned when operator-shaped input was accepted. (No DB dumps or secrets included.)

## 6) Recommended remediation  
- **Enforce strict input types:** cast/validate inputs server-side (e.g., coerce expected strings) before building DB filters.  
- **Reject operator keys in user input:** block or sanitize keys that begin with `$` when accepting client-controlled objects/parameters.  
- **Do not trust parsed query objects:** never pass raw parsed request objects directly into DB driver filters; build explicit filter objects server-side.  
- **Use parameterized/filter APIs:** prefer driver methods that accept typed parameters rather than string-concatenation or dynamic code generation.  
- **Rate-limit and monitor:** detect abnormal sequences of boolean probes (regex-based extraction attempts) with logging/alerts and rate-limiting.  
- **Least privilege for DB accounts:** ensure services have minimal read/write privileges to reduce blast radius.

## 7) Lessons learned  
- NoSQL ≠ no injection — the attacker shifts from "breaking strings" to "injecting operators/structures."  
- Many frameworks parse `field[something]=value` into nested objects; if passed to the DB driver unvalidated, this enables operator injection.  
- `$ne`, `$nin`, `$regex` are powerful logic modifiers that can bypass auth, select different users, or enable blind extraction.  
- Syntax injection (breaking raw JS/query code) exists but is rarer; it requires unsafe string-concatenated query construction.  
- Detection relies on subtle behavioral differences (redirects, different error messages, timing), similar to blind SQLi.

---

## 8) Links / Resources  
- TryHackMe — *Web Application Pentesting* (room)  
- MongoDB docs — querying/filter operators (recommended reading)  
- Notion / detailed write-up (public) — [link to full write-up if published]

---