# El Bandito — TryHackMe / Web Application Pentesting  
**Date:** 2025-11-19  
**Type:** TryHackMe / Challenge  
**Scope:** Lab / Authorized only  

---

## 1) Context
Short summary (1–3 lines):  
Challenge about tracking down **El Bandito** in a DeFi/Web3-themed web app, by exploiting a full chain of web vulns: SSRF → WebSocket request smuggling → Spring Boot Actuator exposure → HTTP/2 → HTTP/1 request desync → message/flag exfiltration.

---

## 2) Initial hypothesis
What I expected before starting:  
- That the challenge would build directly on the previous **HTTP Request Smuggling** rooms (including HTTP/2).  
- That I’d need to combine HTTP/1 and HTTP/2 smuggling/desync tricks with some kind of proxy/Actuator misconfiguration.  
- That following a walkthrough would help me keep track of the multi-bug chain and actually reach the end.

---

## 3) Tools used
Short list — **do not** include options, credentials, or sensitive scripts.  
- Rustscan + Nmap (port and service discovery).  
- Gobuster (web directory/endpoint enumeration).  
- Browser + DevTools (front-end behavior, WebSockets, HTTP/2).  
- Burp Suite (HTTP/1 & HTTP/2 interception, smuggling payloads).  
- Python simple HTTP server + small custom HTTP responder (fake 101 Switching Protocols).  
- Standard Linux CLI (curl, openssl, etc., as needed).

---

## 4) Approach (high level)
Conceptual steps followed (no precise commands):  
- Enumerated open ports (22, 80, 631, 8080) and identified **Spring Boot** behavior on 8080 and a chat app on 80.  
- Used Gobuster on both ports to find interesting endpoints (`/access`, `/ping`, `/trace`, `/token`, `/info`, `/health`, etc.).  
- On 8080, found an **SSRF-style** endpoint (`/isOnline?url=...`) used by a services page and confirmed it would call out to my own HTTP server.  
- Turned this SSRF into a **WebSocket request smuggling primitive** by:  
  - Making the app connect to a controlled HTTP server.  
  - Having that server always reply with a fake **101 Switching Protocols** response.  
  - Embedding a second HTTP/1.1 request behind the upgrade to hit internal Actuator endpoints (e.g., `/trace`).  
- Used `/trace` to discover internal admin paths and alternate internal port (8081), then smuggled requests to retrieve admin credentials and the first flag from an internal admin endpoint.  
- Logged into the front-end chat on port 80 as admin, observed that the app used **HTTP/2**, and identified a message endpoint (`/send_message`) and a hidden `/getMessages` endpoint from the initial source review.  
- Crafted a **HTTP/2 → HTTP/1 request smuggling** payload (zero-length HTTP/2 POST embedding a full HTTP/1 POST with a large Content-Length) so the backend treated the next victim request as part of my smuggled pipeline.  
- Verified successful desync via odd responses (503) and then used `/getMessages` to retrieve other users’ chat messages, including the second flag.

---

## 5) Results / Evidence (sanitized)
What I observed (impact, non-sensitive):  
- The `isOnline` endpoint allowed **SSRF** to arbitrary hosts. By controlling the server response and returning `101 Switching Protocols`, I could desynchronize proxy/backend views of the connection and **smuggle a second HTTP request**.  
- A smuggled request to `/trace` revealed internal Spring Boot Actuator data, including **internal admin endpoints** and an alternate port (8081), demonstrating a classic “debug/ops endpoint exposed behind a proxy” scenario.  
- By smuggling a request to the internal admin endpoints, I obtained working **admin credentials** and retrieved the **first flag** from a protected admin route.  
- Using those credentials on the HTTP/2 front-end chat app (port 80), I could send authenticated admin requests and confirm that Burp was speaking **HTTP/2** to the front-end while the backend still used **HTTP/1.1**.  
- A carefully crafted HTTP/2 desync payload caused the backend to process my **smuggled `/send_message`** request in the wrong context, after which querying `/getMessages` leaked another user’s chat messages and the **second flag**.

---

## 6) Recommended remediation
Generic technical measures (no sensitive details):  
- **Lock down SSRF points** like `isOnline`:  
  - Enforce strict allowlists of hosts/schemes.  
  - Block requests to internal IP ranges, loopback, metadata services, and alternate ports.  
  - Disable or heavily constrain URL-based “health check” features.  
- **Secure or remove Spring Boot Actuator endpoints** (`/trace`, `/env`, etc.):  
  - Expose them only on internal/admin-only networks or behind strong authentication.  
  - Disable debug endpoints in production, or filter them via strict gateway rules.  
- **Harden proxy / WebSocket upgrade chains**:  
  - Ensure all components agree on upgrade behavior.  
  - Normalize or reject unexpected `101` responses from untrusted endpoints.  
  - Add sanity checks to prevent second HTTP requests from being interpreted in WebSocket tunnels.  
- **Mitigate HTTP/2 → HTTP/1 desync**:  
  - Keep front/back parsing logic in sync; apply vendor patches for known h2 desync issues.  
  - Normalize request bodies and Content-Length / Transfer-Encoding before forwarding to backends.  
  - Use independent connection pools per request instead of reusing suspect connections.  
- **General hardening / monitoring**:  
  - Add WAF / RASP rules for suspicious upgrade patterns, malformed `Content-Length`, and tunneling behavior.  
  - Monitor for abnormal error patterns (like frequent 503s) that could signal desync attempts.  

---

## 7) Lessons learned
- **SSRF “health check” endpoints can be way more than connectivity tests.**  
  - Once you control where the server connects, you can often pivot into **protocol tricks** (like WebSocket upgrades) and smuggling.  
- **A fake 101 Switching Protocols response can desync components.**  
  - If a proxy thinks it’s in WebSocket mode while the backend keeps parsing HTTP, everything after the upgrade can diverge → ideal ground for request smuggling.  
- **Spring Boot Actuator endpoints are high-value targets when reachable at all.**  
  - `/trace`, `/env`, and friends can leak internal paths, ports, creds, and flags.  
  - Even if they’re not directly exposed, smuggling your way to them is often possible if there’s a proxy/gateway in front.  
- **HTTP/2 → HTTP/1 desync is very real and very dangerous.**  
  - Typical setup: h2 front, h1 backend.  
  - A 0-length h2 POST containing a full h1 POST and a malicious `Content-Length` can cause the backend to treat your smuggled request as **the next user’s request**.  
- **Error codes (like 503) can be *success signals* when doing desync.**  
  - Instead of ignoring them, they should be treated as indicators that request boundaries got confused.  
- **The real art is chaining bugs, not just finding them.**  
  - Here: SSRF → WS smuggling → Actuator exposure → creds → HTTP/2 desync → data/flag exfiltration.  
  - Keeping a clear mental map (“who talks to who, over what protocol, with which parser?”) is essential.

---

## 8) Links / Resources
- Room / challenge link: *TryHackMe — El Bandito / Bandit Web3 token scam challenge (Web Application Pentesting)*  
- Related THM rooms:  
  - Request Smuggling (Browser / HTTP/1)  
  - Request Smuggling (Secure Proxy Bypass)  
  - Request Smuggling (HTTP/2)  
- Useful docs / reading:  
  - Spring Boot Actuator security docs  
  - PortSwigger Web Security Academy – Request Smuggling labs  
  - Articles on HTTP/2 → HTTP/1 desync vulnerabilities  

---