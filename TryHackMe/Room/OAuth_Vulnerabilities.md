# OAuth Vulnerabilities — TryHackMe - Web Application Pentesting **Date:** 2025-10-20  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context
Learn how the OAuth protocol works and master techniques to exploit it.  
OAuth vulnerabilities emerge as a serious and often overlooked risk in OAuth 2.0 (the widely used authorization framework). Weak configurations can enable CSRF, XSS-assisted theft, data leakage, and other abuses.

---

## 2) Initial Hypothesis
I expected to understand why OAuth 2.0 powers “Login with Google/Facebook,” the roles of each actor (client, resource owner, authorization server, resource server), and how small configuration mistakes escalate into critical vulnerabilities.

---

## 3) Tools Used
- TryHackMe Classroom environment  
- Browser DevTools (Network tab)  
- Burp Suite (traffic interception/inspection)  
- Postman (API testing)  
- jwt.io (token/introspection)

---

## 4) Approach (High Level)
1. Mapped core OAuth concepts (actors, endpoints, tokens, grants).  
2. Traced the **Authorization Code** vs **Implicit** flows in the browser and proxy.  
3. Inspected authorization requests/redirects for `client_id`, `redirect_uri`, `scope`, `state`, and returned `code`/token.  
4. Reviewed token contents (claims, `exp`, `aud`, `iss`) and storage/transport patterns.  
5. Studied common pitfalls: weak `state`, lax redirect URI validation, token exposure in URLs, missing HTTPS/PKCE, long-lived tokens.

---

## 5) Results / Evidence (Sanitized)
- Observed standard OAuth redirects with recognizable parameters (`response_type`, `client_id`, `redirect_uri`, `scope`, `state`).  
- Confirmed how **Authorization Code** exchanges the code server-to-server (safer) vs **Implicit** returning tokens in URL fragments (risk of leakage).  
- Verified the importance of `state`: absent/weak values conceptually enable CSRF-style binding issues.  
- Noted risks when `redirect_uri` validation is permissive (wildcards/mismatches) → potential token/code delivery to attacker-controlled endpoints.  
- Decoded example tokens (no secrets) to see `exp`, `aud`, and `iss` and why enforcing them matters.

---

## 6) Recommended Remediation
- **Strict redirect URI policy:** exact-match registration, no wildcards, enforce HTTPS-only.  
- **PKCE required** for public clients (SPAs/mobile); bind code to the original request.  
- **Mandatory `state`:** cryptographically strong, stored server-side, bound to session, strictly validated on return.  
- **Short-lived access tokens** with **limited scopes**; rotate refresh tokens and bind them to client/context.  
- **Robust token validation:** check `iss`, `aud`, `exp`, and signature on every request.  
- **Secure token handling:** avoid localStorage for SPAs; prefer secure, HttpOnly, SameSite cookies where applicable.  
- **Defense in depth:** logging/monitoring for anomalous redirects/exchanges; replay detection; least-privilege, incremental consent.  
- **Modern defaults:** Prefer **Authorization Code + PKCE**; avoid/deprecate **Implicit**; align with OAuth 2.1 guidance.

---

## 7) Lessons Learned
- OAuth is **authorization**, not authentication; conflating the two creates design flaws.  
- Four actors matter: **Resource Owner**, **Client**, **Authorization Server**, **Resource Server**.  
- **`redirect_uri`** and **`state`** are frequent failure points; minor laxity can compromise the flow.  
- **Implicit Flow** is insecure/deprecated; **Authorization Code + PKCE** is the modern baseline.  
- Tokens are credentials: keep them **short-lived**, **scoped**, and **securely stored**.  
- OAuth 2.1 consolidates safer defaults (PKCE, strict state/redirect handling, better storage practices).

---

## 8) Links / Resources
- TryHackMe: Web Application Pentesting (OAuth module)  
- OAuth 2.0 & 2.1 specs/primers (general)  
- jwt.io (decoder/inspector)  
- Postman / OpenAPI (for testing and docs)

---