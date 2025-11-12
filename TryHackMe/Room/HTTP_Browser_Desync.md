# HTTP Browser Desync — TryHackMe / Web Application Pentesting  
**Date:** 2025-11-12  
**Type:** TryHackMe / Lab  
**Scope:** Lab / Authorized only

---

## 1) Context
Short summary (1–3 lines):  
Hands-on lab to learn **HTTP Request Browser Desync** (browser-side request smuggling). Objective: understand how connection reuse + framing inconsistencies allow an attacker to have previously-sent bytes executed in a victim’s request context, and how that can be chained to achieve session hijack / XSS / cache poisoning.

## 2) Initial hypothesis
What I expected before starting:  
- Browser desync is a variant of request smuggling where only the front-end parsing needs to be desynchronized.  
- If framing differs between components (proxy / backend / browser), attacker bytes can be applied to a victim’s request.  
- In a controlled lab, it should be possible to seed a connection and trigger the victim to fetch attacker-controlled content, enabling session actions or token exfiltration.

## 3) Tools used
Short list:  
- Browser DevTools (Network, Console)  
- curl (sanity probes)  
- Simple Python HTTP server (testing / canary)  
- Basic JS `fetch()` from console / HTML form gadget  
— **do not** include options, credentials, or sensitive scripts.

## 4) Approach (high level)
Conceptual steps followed:  
- Analyze application endpoints to find inconsistent behavior (which endpoints reflect/interprete input vs which only echo).  
- Attempt to seed a pooled connection with a crafted request (H1-in-body or form seed) that survives intermediary normalization.  
- Trigger a victim-like request (via browser refresh or a second tab) to cause the server/proxy to treat the queued bytes as the victim’s request.  
- Host a tiny benign canary server to verify that the victim fetches attacker-hosted content (no sensitive exfiltration performed).  
- Use curl probes to check for normalization / framing acceptance by the target stack.

## 5) Results / Evidence (sanitized)
What I observed:  
- Evidence of desync indicators: repeated refreshes and crafted seeds produced an unexpected 404 from the server — sign of framing/parsing anomalies.  
- One endpoint reflected input without interpreting it; another reflected and interpreted payloads differently — indicating inconsistent parsing across endpoints.  
- Canary fetch confirmed the victim browser fetched attacker-served JS when the connection was seeded and the victim triggered a request; final state: benign canary ping observed (no secrets exfiltrated).  
- Curl edge-case probe (Content-Length vs Transfer-Encoding) produced different server behavior suggesting permissive framing normalization — actionable indicator for request-smuggling risk.

## 6) Recommended remediation
Generic technical measures:  
- Enforce strict request framing at the earliest hop (normalize or reject inconsistent `Content-Length` / `Transfer-Encoding` combinations).  
- Disable or tightly control connection reuse for sensitive endpoints (or ensure per-request isolation in front-end components).  
- Apply idempotency keys / per-request tokens where appropriate to avoid treating queued bytes as separate authenticated actions.  
- Ensure caches are keyed safely (avoid caching responses that can be influenced by request body bytes) and set safe cache-control headers for dynamic endpoints.  
- Add input validation and canonicalization consistently across all components (proxy, load-balancer, backend) and run fuzzing tests for ambiguous framing.

## 7) Lessons learned
- **Browser desync ≠ classic smuggling:** only desynchronizing the front-end/parser can be enough to hijack a victim’s request context.  
- **Same-origin behavior matters:** SameSite/CORS rarely block cookie transmission when the request is perceived as same-origin — cookie protections don’t prevent the described attack flow.  
- **Precision is critical:** correct `\r\n` boundaries and minimal deterministic payloads are required; tooling that auto-rewrites requests can invalidate tests.  
- **Chain small, verifiable steps:** seed → trigger → fetch → execute (or fail). Validate each step with benign canaries to avoid unsafe testing.  
- **Observability helps:** use two tabs (attacker / victim) and watch Network + Console to confirm connection reuse and the canary fetch.

---

## 8) Links / Resources
- TryHackMe — Web Application Pentesting (classroom / lab)  
- Useful docs: request smuggling / HTTP framing (Content-Length vs Transfer-Encoding), browser connection pooling articles, canonical mitigation guidance.  
- Notion / detailed write-up (public): _[add link here if published]_  

---