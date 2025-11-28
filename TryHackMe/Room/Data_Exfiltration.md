# Data Exfiltration — TryHackMe - Red Team
| --- | --- |
| Type | Classroom |
| Statut | Done |
| Date | 27/11/2025 |

# Context

An introduction to **Data Exfiltration and Tunneling techniques** across multiple protocols.  
Objective: understand how attackers move stolen data out of a corporate environment while avoiding detection, and how those same techniques enable stealthy C2 and covert tunneling.

A complete walkthrough of exfiltration over:
- TCP sockets  
- SSH  
- HTTP/HTTPS  
- ICMP  
- DNS  
- DNS Tunneling (iodine)  
- DNS-based C2 (TXT records)  
- HTTP Tunneling (Neo-reGeorg)

---

# Starting hypothesis

Explore data exfiltration, C2 channels, and tunneling over legitimate protocols.

- What is data exfiltration?
- How do attackers adapt exfiltration to different protocols?
- How encoding, chunking, and protocol abuse allow stealthy data flow.
- How C2 and tunneling extend exfiltration into **bidirectional communication**.
- Practise tunneling over DNS and HTTP to pivot inside networks.

---

# Method / Used Tools

### Protocols & Techniques
- Raw TCP sockets (netcat, /dev/tcp)
- SSH streaming & SCP-less transfers
- HTTP/S POST exfiltration (curl + PHP handler)
- HTTP tunneling with **Neo-reGeorg**
- ICMP manual + automated (Metasploit `icmp_exfil`)
- ICMP C2 (ICMPDoor)
- DNS exfiltration (A queries)
- DNS TXT-based C2
- DNS tunneling using **iodine** + SSH dynamic port-forwarding
- ProxyChains over DNS tunnel
- tcpdump for monitoring exfiltration streams

### Encodings
- base64, hex (xxd)
- gzip/tar streaming
- EBCDIC/ASCII transforms
- DNS-safe chunking (≤63 chars per label, ≤255 chars per FQDN)

---

# Approach (High-Level)

### 1. **Data Exfiltration Fundamentals**
- Defined traditional exfiltration vs C2 vs tunneling.
- Analyzed protocol constraints, normal behavior, and detection surfaces.
- Built a mental framework: **stealth ↔ bandwidth ↔ reliability**.

### 2. **TCP Socket Exfil**
- Simple listener on JumpBox using `nc -lvp`.
- Streamed tar → base64 → EBCDIC via `/dev/tcp/IP/port`.
- Reconstructed with dd (EBCDIC→ASCII) + base64 -d + tar.

### 3. **SSH Exfiltration**
- Tar stream piped directly into remote SSH command.
- Zero artefacts on victim disk.
- Fully encrypted transport over port 22.

### 4. **HTTP(S) Exfiltration**
- Built a minimal PHP POST receiver.
- Encoded directory → base64 → curl POST.
- Corrected URL-encoding corruption (sed replace space→+).
- Extracted with base64 -d + tar.
- Practised **Neo-reGeorg**: built a full HTTP tunnel → SOCKS proxy.

### 5. **ICMP Exfiltration**
- Manual `ping -p <hex>` payloads.
- Automated Metasploit `auxiliary/server/icmp_exfil` with BOF/EOF markers.
- ICMPDoor reverse shell for C2 via ICMP echo requests/replies.

### 6. **DNS Configuration & Testing**
- Modified Netplan to set custom nameserver.
- Verified DNS resolution for controlled zone.
- Prepared authoritative zone for `*.tunnel.com`.

### 7. **DNS Exfiltration**
- Encoded file → base64 → chunked ≤18 chars.
- Embedded chunks as DNS labels.
- Exfiltrated via `dig` queries.
- Reconstructed by extracting base64 chunks from tcpdump and decoding.

### 8. **DNS TXT-based C2**
- Encoded script into base64.
- Inserted in TXT record (`script.tunnel.com`).
- Retrieved via `dig TXT` → base64 -d → piped directly to `bash`.

### 9. **DNS Tunneling (iodine)**
- Started iodined server (`10.1.1.1/24`).
- Connected victim via iodine client (`10.1.1.2`).
- Created full virtual interface (`dns0`).
- Established SSH dynamic port forwarding (-D 1080).
- Used ProxyChains to access internal hosts **entirely over DNS**.

---

# Results / Evidence (Sanitized)

- Successfully exfiltrated directories and files via TCP, SSH, HTTP, ICMP, and DNS.
- Built covert bidirectional tunnels using HTTP (Neo-reGeorg) and DNS (iodine).
- Achieved full C2 over ICMP and DNS TXT responses.
- Proved that DNS tunneling enables internal network pivoting through restrictive firewalls.
- Confirmed protocol-specific encoding/transport constraints (FQDN length, ICMP payload size, HTTP POST limits).
- All data reconstructed without storing plaintext artefacts on victims.

*(No credentials, IPs, or real data included.)*

---

# Issue met

The main challenge was **maintaining a unified mental model** as the room switched across drastically different protocols and threat models.

Each protocol changes:
- what “normal” traffic looks like
- what defenders typically monitor
- which encoding survives transport
- how much data can be moved
- whether communication supports one-way exfil or full bidirectional C2/tunneling

Understanding the *tradeoff triangle* (stealth ↔ bandwidth ↔ reliability) was key to internalizing it.

---

# Solution

To consolidate understanding, I treated each protocol as a separate module, then linked them conceptually.

### **TCP**
- High bandwidth, low stealth.

### **SSH**
- Cleanest, safest, most legitimate exfil when allowed.

### **HTTP(S)**
- The attacker’s “default” channel: ubiquitous, encrypted, flexible.

### **ICMP**
- Small, stealthy, firewall-friendly channel for C2 and small data theft.

### **DNS**
- The ultimate covert channel:
  - exfiltration
  - payload delivery
  - C2
  - tunneling
  - beaconing

### **DNS Tunneling (iodine)**
- A full VPN-like experience inside DNS.
- Enables pivoting, scanning, RDP/SMB/LDAP over DNS queries.

### **Final Integration**
By testing each technique in isolation and then chaining them, I built a coherent picture of:
- when attackers choose each protocol
- how they combine them
- how encoding transforms incompatible data into valid protocol payloads
- how stealth relies on blending into legitimate traffic patterns

---

# What I have learned

- **Exfiltration = deception**, not just file transfer.
- Encoding (base64, hex, EBCDIC) is essential for protocol compatibility.
- DNS/ICMP/HTTPS are rarely inspected deeply → high-value escape channels.
- **C2 = exfil + inbound tasking**, using the same channels.
- DNS TXT responses can deliver scripts and commands invisibly.
- DNS tunneling acts like a **VPN over DNS**, enabling:
  - proxying
  - scanning
  - internal browsing
  - lateral movement setup
- Stealth depends on matching normal protocol behavior, not just encryption.
- Real attackers chain techniques:  
  **DNS tunnel → SOCKS → SMB → SSH exfil**.

---

# Takeaways

- **DNS is the Swiss Army knife of covert channels**  
  Exfil, C2, tunneling, payloads, beaconing.

- **ICMP is underrated but powerful**  
  Great for low-noise C2 and small secrets.

- **HTTPS dominates real-world exfiltration**  
  High bandwidth, encrypted, baseline-expected.

- **SSH is ideal when accessible**  
  Legitimate admin traffic, encrypted, fast.

- **Encoding is the glue that holds all covert channels together**.

- **Pivoting ≠ lateral movement**  
  - Pivoting = expanding *reach*  
  - Lateral movement = expanding *control*  
  Exfiltration + tunneling often sit between both.

- **Mastering exfiltration = mastering post-exploitation freedom.**

---