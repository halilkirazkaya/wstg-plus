## WSTG-BUSL-07 — Test Defenses Against Application Misuse

Defenses against application misuse evaluate the application's ability to identify, log, and respond to anomalous user behavior that deviates from intended business logic. Unlike traditional signature-based security, these defenses focus on detecting patterns such as rapid-fire requests, credential stuffing, or sequential resource enumeration that signify automated abuse. From an attacker's perspective, this test determines the resilience of the application against automation and "low and slow" attacks designed to exfiltrate data or exhaust resources without triggering standard security alerts. Failure to implement these defenses often results in massive data scraping, account takeovers, or denial of service through legitimate but resource-intensive application features.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Are rate-limiting or throttling mechanisms enforced on sensitive endpoints?
- [ ] Yes — strict rate limiting **is applied** and enforced across all sensitive endpoints *(Most secure)*  
- [ ] Yes — rate limiting **is applied** but can be bypassed via IP rotation or header manipulation  
- [ ] Yes — rate limiting **is applied** only to authentication endpoints, leaving others exposed  
- [ ] No — no rate limiting or throttling **is applied** to any application features  

### Does the application detect and block automated interaction patterns?
- [ ] Yes — behavioral analysis or CAPTCHAs **are enabled** and effectively block bots  
- [ ] Yes — anti-automation **is enabled** but bypass **is possible** using headless browsers or session cycling  
- [ ] No — application **cannot** distinguish between a human user and an automated script  

### Is anomalous behavior logged and followed by a defensive response?
- [ ] Yes — misuse triggers automated blocking and alerts the security team  
- [ ] Yes — misuse is logged for audit but **no** automated defensive action is taken  
- [ ] No — the application **does not** log or respond to unusual activity patterns  

### Can defenses be bypassed by manipulating client-side identifiers or headers?
- [ ] No — defenses rely on server-side state and verified telemetry  
- [ ] Yes — controls **can** be bypassed by clearing cookies or rotating `User-Agent` strings  
- [ ] Yes — controls **can** be bypassed by injecting headers like `X-Forwarded-For` or `X-Real-IP`  

---