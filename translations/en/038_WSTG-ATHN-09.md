## WSTG-ATHN-09 — Testing for Weak Password Change or Reset Functionalities

Password change and reset mechanisms are critical components of authentication management that, if improperly implemented, allow attackers to take over user accounts. These vulnerabilities typically involve predictable reset tokens, lack of account lockouts during brute-force attempts, insecure delivery of tokens, or the ability to change passwords without verifying the current password. Attackers exploit these flaws by intercepting or guessing recovery links, performing host header injection to redirect reset emails, or utilizing session fixation to maintain access after a password change. Ensuring these processes require strong verification and utilize cryptographic randomness is essential for maintaining account integrity and preventing unauthorized access.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Is the current password required to set a new password during a standard change?
- [ ] Yes — current password **is required** and verified  
- [ ] Yes — current password **is required** but can be bypassed via parameter pollution or manipulation  
- [ ] No — current password **is not** requested, allowing account takeover via session hijacking *(High)*  

### Are password reset tokens cryptographically secure and unpredictable?
- [ ] Yes — tokens are high-entropy, random, and **not predictable**  
- [ ] Yes — tokens are generated but show low entropy or predictable patterns (e.g., timestamp-based)  
- [ ] No — tokens are **predictable** or based on static user data like Base64 encoded email/ID *(Critical)*  

### Can the password reset link be redirected via Host Header Injection?
- [ ] No — application ignores or validates the `Host` header for link generation  
- [ ] Yes — application uses the `Host` or `X-Forwarded-Host` header to construct links, but validation **is present**  
- [ ] Yes — links **can** be redirected to an attacker-controlled domain via header manipulation *(High)*  

### Does the reset token have a limited lifecycle and immediate invalidation?
- [ ] Yes — tokens expire quickly and are invalidated **immediately** after use or password change  
- [ ] Yes — tokens expire after a long duration (e.g., 24+ hours) or allow **multiple uses**  
- [ ] No — tokens **never expire** or remain valid after the password has been successfully changed  

### Are there rate limits or lockouts on the password reset and change endpoints?
- [ ] Yes — strict rate limiting and CAPTCHAs **are applied** to prevent enumeration and brute-force  
- [ ] Yes — limited controls exist but bypass **is possible** via IP rotation or header spoofing  
- [ ] No — no rate limiting **is applied**, allowing for bulk account enumeration or token brute-forcing  

---