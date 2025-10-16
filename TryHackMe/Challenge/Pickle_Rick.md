# Pickle Rick — TryHackMe Web Fundamentals
**Date:** 2025-10-16  
**Type:** Challenge  
**Scope:** Lab / Authorized only

---

## 1) Context
Short CTF inside the *Web Fundamentals* track. Minimal site with pop-culture hints; goal is to recover three “ingredients” (flags) and “turn Rick back into a human.” Focus on practical enumeration, hidden endpoints, and privilege/command constraints post-login.

## 2) Initial hypothesis
- Homepage + Easter-egg strings ⇒ creds likely in **source/robots**.
- Hidden login endpoint (likely **`.php`**) to a simple **command panel**.
- Post-login shell with restricted bins; a **sudo misconfig** may allow reading any flag.

## 3) Tools used
- Browser + View-Source / DevTools
- Burp Suite (intercept/replay)
- `nmap` (port sweep), `curl` (quick checks)
- Gobuster / ffuf (dir discovery)
- Shell viewers: `less`, `more`, `head`, `tail`, `awk`, `sed`, `dd`, `base64`
- POSIX search: `find`, `grep`
- `sudo` (rights inspection)

> No options, tokens, or real credentials stored here.

## 4) Approach (high level)
- **Recon:** Load homepage, view source, check `robots.txt`, fingerprint stack hints.
- **Wordlist probing:** Try common paths with and without extensions; add `.php`.
- **Auth:** Test CTF-style creds surfaced via comments/robots.
- **Post-login:** Use allowed readers (`less`, `head`, …) to view clues/flags since `cat` is blocked.
- **Privilege check:** Run `sudo -l` early; if permissive, use `sudo` to read flags under `/root` and user homes.
- **Targeted search:** Prefer `find`/`grep` over manual `ls` walks for faster flag discovery.

## 5) Results / Evidence (sanitized)
- **Source comment** leaked a username: `R1ckRul3s` (CTF-only).
- **robots.txt** exposed a show reference used as password: `Wubbalubbadubdub` (CTF-only).
- **Hidden endpoint:** `/portal.php` accepted those creds → **command panel** as `www-data`.
- **Binary restrictions:** `cat` disabled; alternative viewers worked to read files.
- **Flags:**
  - `clue.txt` (first ingredient + hint to explore FS).
  - `/home/rick/"second ingredients"` (second ingredient).
  - `/root/3rd.txt` via `sudo` (third ingredient).
- **Key behaviors to note (for remediation):**
  - Secrets in **comments/robots**.
  - Admin panel hidden only by **obscurity**.
  - `www-data` had **over-permissive sudo** for read operations.

> Screens/requests kept locally; no sensitive tokens included here.

## 6) Recommended remediation
- **Credential hygiene:** Never store secrets in HTML comments or `robots.txt`. Use vaults; rotate secrets; enforce MFA.
- **Endpoint exposure:** Don’t rely on obscurity. Put admin panels behind auth (and ideally IP allow-lists/VPN), add CSRF, rate-limit, and bot checks.
- **Dir discovery hardening:** Serve **consistent 404s** and avoid content-length leaks; disable directory listing.
- **Least privilege:** `www-data` must have **no sudo**. Separate users for web and maintenance; lock down file permissions; mount upload/log dirs as **noexec,nodev,nosuid** where possible.
- **Command panels:** Avoid arbitrary command execution features; if necessary, whitelist sub-commands in a brokered service, not a shell.
- **Secrets management:** Remove hard-coded credentials; add CI checks that fail on secret patterns in static assets.
- **Monitoring:** Alert on requests to odd admin paths, brute-force patterns, and access to `robots`-listed disallows.

## 7) Lessons learned
- **Try extensions** when fuzzing (`-x php,html,txt,bak`), or you’ll miss classic `.php` panels.
- **Use multiple wordlists** (SecLists `common`, `raft-*`) and filter noisy 404s (by status + length/body).
- **`sudo -l` early**—can save long enumeration if privileges are loose.
- **Targeted search beats guessing:** `find` / `grep -RIl` across `/home`, `/var/www` is faster than recursive `ls`.
- **When a binary is blocked,** many equivalents exist (`head`, `awk '1'`, `sed -n '1,$p'`, `base64 | base64 -d`, `dd`).

## 8) Links / Resources
- TryHackMe — *Web Fundamentals* (room with Rick & Morty mini-CTF)
- SecLists (wordlists for web discovery)
- Gobuster & ffuf documentation
- OWASP Testing Guide — Information Gathering & Auth Testing

---