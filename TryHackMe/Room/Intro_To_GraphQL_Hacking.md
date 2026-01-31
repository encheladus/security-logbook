# Intro to GraphQL Hacking — TryHackMe

**Date:** 2026-01-29  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only

---

## 1) Context

Short summary (1–3 lines):

Introductory lab focused on understanding GraphQL fundamentals, schema discovery, and common security pitfalls.  
Objective: learn how GraphQL differs from REST and how misconfigurations can lead to data exposure, injection, and DoS risks.

---

## 2) Initial hypothesis

- GraphQL introspection may be exposed and allow full schema enumeration.
- Weak or missing field-level authorization could lead to excessive data exposure.
- User-controlled input in resolvers may be vulnerable to injection.
- Lack of query limits could enable resource exhaustion via complex queries.

---

## 3) Tools used

- Browser DevTools
- Burp Suite
- GraphiQL-style queries
- curl

---

## 4) Approach (high level)

- Identify the GraphQL endpoint through application behavior and common paths.
- Explore schema structure using introspection mechanisms.
- Analyze queries and mutations to understand available fields and relationships.
- Test how input is handled by resolvers and how much data can be requested.
- Observe behavior when crafting complex or deeply nested queries.

---

## 5) Results / Evidence (sanitized)

- Full schema was accessible through unauthenticated introspection.
- Queries returned more fields than necessary, including sensitive attributes.
- Inputs were passed to backend logic without strict validation.
- No apparent limits on query depth or complexity were enforced.

Final state observed:  
API allowed excessive visibility and flexible query execution without adequate safeguards.

---

## 6) Recommended remediation

- Disable GraphQL introspection in production or restrict it to authorized roles.
- Enforce field-level authorization within resolvers.
- Use parameterized queries for all database interactions.
- Validate and sanitize all user-supplied inputs.
- Apply query depth and complexity limits.
- Implement request rate limiting.

---

## 7) Lessons learned

- GraphQL is powerful but insecure by default.
- Introspection acts as a full API blueprint for attackers.
- Field-level security is critical; global auth is not sufficient.
- A single vulnerable resolver can expose the entire backend.
- GraphQL security testing is schema-driven, not endpoint-driven.

---

## 8) Links / Resources

- TryHackMe – Intro to GraphQL Hacking  
- GraphQL Official Documentation  
- Public write-up (if published)