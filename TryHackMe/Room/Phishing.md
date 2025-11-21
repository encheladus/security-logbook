# Phishing — TryHackMe / Red Team 
**Date:** 2025-11-21  
**Type:** Classroom (theory + simulated practice)  

---

## 1) Context

Room about **phishing as an initial access vector** in red team engagements:

- What phishing is and where it fits in an engagement.
- How to set up **phishing infrastructure** (domain, SMTP, landing pages).
- How to craft **convincing emails** and measure results.
- How payloads (droppers, macros, browser exploits) plug into the phishing chain.

---

## 2) Initial hypothesis / expectations

- Understand how “real” phishing is done in professional red teaming.
- See concrete tools and workflows (e.g. **GoPhish**).
- Realize how HTTP infrastructure + weaponization + social engineering join into a full chain.

---

## 3) Tools / components involved

Conceptual + practical:

- **Infrastructure**
  - Domain names (fresh, expired, typosquatted, homograph).
  - DNS: **SPF / DKIM / DMARC**.
  - Web server + HTTPS (TLS certs).
  - SMTP / mail server or outbound provider.

- **Frameworks**
  - **GoPhish** (sending profiles, templates, landing pages, analytics).
  - **SET (Social Engineering Toolkit)** for cloning sites / spear-phish.

- **Payloads / Delivery**
  - **Droppers** (lightweight “installers” that fetch real malware).
  - **Office documents + macros** (VBA).
  - **Browser exploits** (rare but possible, esp. legacy).
  - Ties back to previous rooms: HTA, PowerShell, C2 implants.

---

## 4) Process I followed (high level)

Mentally, the room walks you through the full phishing chain:

1. **Understand the human angle**
   - Phishing as social engineering: playing on urgency, fear, reward, routine, “corporate normality”.
   - Difference between **mass phishing** and **spear-phishing**.

2. **Design the email**
   - **Sender**: OSINT to pick a believable persona or brand.
   - **Subject**: urgency + relevance (password reset, invoice, HR request, shipment, etc.).
   - **Body**: mimic real templates — tone, logo, layout, signature, links.

3. **Build infrastructure**
   - Register / choose domain (expired, typo, alt TLD, or IDN lookalike).
   - Set up:
     - DNS (A, MX, SPF/DKIM/DMARC).
     - Web server + TLS cert.
     - SMTP / sending account.

4. **Use GoPhish to operationalize**
   - Create **Sending Profile** (SMTP).
   - Create **Landing Page** (spoofed login / form).
   - Create **Email Template** (with embedded tracking and links).
   - Create **Targets Group**.
   - Launch **Campaign** and track:
     - Delivered / opened / clicked / data submitted.

5. **Connect to weaponization**
   - Landing page can:
     - Harvest credentials.
     - Serve **droppers**, macro docs, or HTA/PowerShell launcher.
   - That payload → **C2** → full compromise.
   - Browser exploits are a niche but powerful variant (especially in legacy environments).

---

## 5) Key things I learned

From your notes, distilled:

- **Phishing is mostly psychology, not magic tech.**
  - The “vulnerability” is human trust & routine.
  - Technical pieces just support the illusion (domain, TLS, logos).

- **Convincing emails are all about detail:**
  - Correct sender name / pattern (`firstname.lastname@...`).
  - Realistic subject line (matches the person’s role and current context).
  - Authentic formatting (logo, footer, disclaimers, wording).

- **Infrastructure is not hard, just methodical:**
  - With a decent domain + SPF/DKIM/DMARC + HTTPS, you’re already playing in the “legit” league.
  - Tools like **GoPhish** remove most friction.

- **Droppers and macros complete the kill chain:**
  - Email → click → dropper/macro → payload → C2.
  - Droppers are often **non-obvious** to AV because they look simple and small.

- **Domain choice is a psychological weapon:**
  - Expired domains = existing reputation.
  - Typosquatting, alt TLDs, IDN homographs = “looks right at a glance”.

- **Office docs are still king as attachments:**
  - People are used to opening them.
  - “Enable Content / Macros” is still a massively exploited pattern.

- **Browser exploits are rare but important in legacy environments.**
  - Not your default tool, but useful where old IE/Edge/plug-ins still exist.

- **Big realization:**  
  You don’t need to be ultra-technical to be dangerous with phishing.
  You need to be **observant, sneaky and good at mimicking “normal”**.

---

## 6) Defensive / blue-team angles (summary)

What this implies for defenders:

- Harden **email**:
  - Proper SPF, DKIM, DMARC.
  - Attachment and URL analysis, sandboxing.
  - Flag external senders clearly.

- Harden **browsers & Office**:
  - Keep browsers updated; remove legacy where possible.
  - Macro restrictions: block internet-origin macros; enforce signed macros.
  - Disable or restrict `mshta`, legacy scripting engines, etc.

- Harden **users**:
  - Regular, realistic phishing simulations.
  - Teach people to check domains, hover links, question unexpected attachments / urgencies.
  - Normalize “verify out-of-band” (call the person, use a separate channel).

- Harden **endpoint & network**:
  - Monitor LOLBIN usage (PowerShell, mshta, wscript).
  - EDR rules around unusual child processes from Office / mail clients.
  - Detect abnormal outbound connections after macro execution.

---

## 7) Personal takeaway

> Phishing is **shockingly accessible** once you understand the basic building blocks:  
> - believable email  
> - realistic infrastructure  
> - simple payload  
>  
> The “hard” part isn’t the code — it’s thinking like the target and making the whole thing feel boringly normal.

---