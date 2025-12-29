# Tooling via Browser Automation - Web Application Red Teaming — TryHackMe
**Date:** 2025-12-28  
**Type:** Classroom  
**Scope:** Lab / Authorized only

---

## 1) Context
Short summary: This room focuses on creating custom tooling for web application testing using **Selenium** and **Playwright**. The goal is to understand how browser automation can be leveraged for red teaming, including brute-force attacks, CAPTCHA bypassing, and dynamic vulnerability scanning.  

Browser automation allows software to interface with a browser like a human, executing JavaScript and updating the DOM as requests are made. This approach reduces the need to reverse-engineer client-side logic and provides realistic exploitation paths for complex, stateful workflows.

---

## 2) Initial hypothesis
- Learn how Selenium works and can be used to create custom tools and exploits.  
- Understand considerations when using browser automation for offensive testing.  
- Create a custom Selenium script capable of brute-forcing CAPTCHAs.  

---

## 3) Tools used
- Selenium WebDriver  
- selenium-stealth  
- Playwright  
- OWASP ZAP  
- fake_useragent  
- Pillow (PIL)  
- Tesseract OCR  

---

## 4) Approach (high level)
1. Analyze the target workflow and identify where browser-side logic is critical.  
2. Use Selenium to automate login attempts, handle CSRF tokens, and perform stealth configuration.  
3. Preprocess CAPTCHA images and perform OCR to simulate human input.  
4. Leverage Playwright to navigate complex workflows and route traffic through OWASP ZAP for passive vulnerability scanning.  
5. Validate results using browser state rather than raw HTTP responses.  
6. Combine automation, stealth techniques, and proxy analysis to create realistic red team tooling.  

---

## 5) Results / Evidence (sanitized)
- Selenium successfully executed login brute-force attacks without reverse-engineering JS logic.  
- CAPTCHA bypass achieved using visual extraction and OCR.  
- Playwright + OWASP ZAP combination allowed passive vulnerability scanning of JavaScript-heavy workflows.  
- Scripts operated inside a real browser, preserving session state, CSRF tokens, and client-side protections.  
- Proof of concept: successful login messages and automated extraction of dashboard content.  

---

## 6) Recommended remediation
- Implement server-side rate-limiting for login attempts.  
- Employ advanced CAPTCHA mechanisms that combine behavioral and visual challenges.  
- Enforce multi-factor authentication for critical endpoints.  
- Validate all client inputs on the server side to prevent automation-driven abuse.  
- Monitor and alert for unusual patterns indicative of automated tools, even if executed via a browser.  

---

## 7) Lessons learned
- Browser automation is a powerful alternative to client-side reverse engineering.  
- Running attacks inside a real browser simplifies handling JavaScript, CSRF, and dynamic tokens.  
- CAPTCHA and client-side protections are significantly weakened when automation mimics human interaction.  
- Stealth techniques and fingerprinting considerations are essential for realistic automation.  
- Modern red teaming blends automation, tooling, and application logic awareness.  

---

## 8) Links / Resources
- [TryHackMe Room](https://tryhackme.com)  
- [Selenium Documentation](https://www.selenium.dev/documentation/)  
- [Playwright Documentation](https://playwright.dev/)  
- [OWASP ZAP](https://www.zaproxy.org/)  
- [Notion / Detailed Write-up (Public)](https://www.notion.so)  

---