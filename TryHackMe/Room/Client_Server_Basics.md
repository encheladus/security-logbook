# Client-Server Basics — TryHackMe (Computer Science)

**Date:** 2026-02-21  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room introduces the fundamentals of the client-server model and core networking concepts such as DNS, ports, protocols, and HTTP.  
The objective was to understand how modern network communication works and how web applications rely on structured request/response exchanges.

---

## 2) Initial hypothesis

Before starting, I expected to:

- Clearly understand the client-server model  
- Grasp the surface-level definitions of:
  - DNS  
  - Client  
  - Server  
  - Port  
  - Protocol  
  - Network  
- Understand how HTTP communication works in practice  

---

## 3) Tools used

- Web browser  
- TryHackMe interactive lab environment  

---

## 4) Approach (high level)

I approached the room conceptually by breaking everything into layers and analogies.

First, I used the pizza delivery analogy to understand the client-server interaction model:

- The client always initiates the request  
- The server waits passively and responds  
- The service is the resource being provided  

Then, I decomposed web communication into logical layers:

- Network layer → IP address identifies the host  
- Transport/service layer → Port identifies the specific service  
- Application layer → Protocol (HTTP, FTP, SMTP) defines communication rules  
- Application logic → Sessions and cookies maintain state  

I studied HTTP structure in practice:

- A request contains:
  - Method  
  - Path  
  - Headers  
  - Optional body  
- A response contains:
  - Status code  
  - Headers  
  - Body  

I also focused on understanding statelessness in HTTP and how session identifiers (cookies or tokens) are used to simulate persistent authentication.

---

## 5) Results / Evidence (sanitized)

After completing the room, I was able to:

- Clearly differentiate between client, server, and service  
- Explain how DNS translates domain names into IP addresses  
- Understand how IP + port uniquely identifies a specific service  
- Describe HTTP as a stateless protocol  
- Explain how sessions are maintained via cookies or tokens  
- Identify the purpose of core HTTP methods (GET, POST, PUT, DELETE, etc.)  
- Understand the structure of HTTP requests and responses  

Final state: clear mental model of how web communication flows from client to server and back.

---

## 6) Recommended remediation

From a defensive perspective, understanding these basics highlights important security practices:

- Restrict unnecessary open ports  
- Disable unused HTTP methods (e.g., PUT, DELETE if not required)  
- Properly configure DNS records  
- Protect session identifiers (Secure, HttpOnly cookies)  
- Avoid exposing sensitive information in HTTP headers  
- Enforce HTTPS to protect data in transit  

---

## 7) Lessons learned

- The client-server model is the backbone of modern networking  
- HTTP is stateless by design; state persistence is implemented at the application layer  
- IP identifies the machine, port identifies the service, DNS resolves human-readable names  
- HTTP methods represent intent and can introduce risk if misconfigured  
- Understanding request/response structure is fundamental for future pentesting work  

---

## 8) Links / Resources

- Room: TryHackMe – Computer Science (Client-Server Basics)  
- RFC documentation (HTTP specification)  
- Personal Notion notes (detailed write-up)

---