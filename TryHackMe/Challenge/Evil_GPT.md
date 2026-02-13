# Evil-GPT — TryHackMe LLM

**Date:** 2026-02-11  
**Type:** TryHackMe Challenge  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room simulates a remote AI-powered command interface accessible via netcat.  
The service translates natural language instructions into Linux commands and asks for confirmation before execution.

The objective was to retrieve the flag from the system while understanding how the AI command intermediary behaves.

---

## 2) Initial hypothesis

Before starting, I assumed:

- The flag would likely be stored in a privileged directory such as `/root`.
- Certain commands might be restricted.
- The AI layer could filter, modify, or reinterpret user input before execution.
- The system might not behave as a traditional interactive shell.

---

## 3) Tools used

- Netcat  
- Basic Linux command knowledge  

(No sensitive commands or credentials included.)

---

## 4) Approach (high level)

1. Connected to the remote service.
2. Began enumerating common directories.
3. Attempted direct command execution.
4. Observed abnormal command behavior.
5. Identified the presence of a natural language → command translation layer.
6. Adapted interaction strategy to leverage natural language requests instead of raw shell commands.
7. Confirmed and executed the AI-generated command to retrieve the flag.

---

## 5) Results / Evidence (sanitized)

During enumeration:

- Standard directory exploration did not immediately reveal useful information.
- Direct raw shell commands were sometimes modified, blocked, or misinterpreted.
- Certain commands (e.g., directory navigation attempts) were not handled as expected.

Key observation:

When providing structured natural language requests instead of raw commands, the AI generated valid Linux commands that were successfully executed.

Final state:

- The `/root` directory was accessible through AI-generated commands.
- The file `flag.txt` was readable.
- Flag successfully retrieved.

---

## 6) Recommended remediation

To secure such a system, the following measures should be implemented:

- Enforce strict command allowlists at execution level.
- Avoid direct system command execution from AI-generated outputs.
- Use sandboxed environments with restricted permissions.
- Apply proper role-based access control.
- Validate and sanitize generated commands before execution.
- Avoid relying solely on superficial command filtering (e.g., blocking specific keywords).

---

## 7) Lessons learned

- AI systems that translate natural language into system commands introduce a new attack surface.
- Blocking specific commands does not secure a system if alternative dangerous commands remain accessible.
- Security must be enforced at the execution layer, not only at the input layer.
- Understanding the system architecture is critical before attempting exploitation.
- Prompt engineering can influence command generation and bypass weak filtering logic.
- AI-based interfaces can be vulnerable to logic abuse and indirect command execution.

---

## 8) Links / Resources

- TryHackMe Room: LLM (Evil-GPT)
- Documentation on prompt injection concepts
- Notes on AI command execution security
- Personal Notion write-up (public version)

---