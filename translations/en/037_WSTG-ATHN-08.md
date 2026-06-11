## WSTG-ATHN-08 — Testing for Weak Security Question Answer

Testing for weak security question answers involves evaluating the knowledge-based authentication (KBA) mechanisms used to verify a user's identity, typically during password recovery or multi-factor authentication flows. This matters because security questions often rely on static, non-secret information that can be easily discovered through open-source intelligence (OSINT) or social engineering, leading to unauthorized account takeover. These vulnerabilities typically occur in the "Forgot Password" or "Account Unlock" modules where users provide answers to pre-set questions. From an attacker's perspective, this is a prime target for automated brute-forcing or targeted research, as many users provide predictable or publicly verifiable answers to common questions like "What is your mother's maiden name?" or "What high school did you attend?".


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the security question is the sole requirement for a full password reset and account takeover without further verification.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Does the application implement security questions for identity verification or password recovery?
- [ ] No — security questions are **not used** for authentication or recovery  
- [ ] Yes — security questions are **used** as an optional secondary factor  
- [ ] Yes — security questions are **used** as the primary/sole recovery mechanism *(High Risk)*  

### Are security questions selected from a fixed list or are they user-defined?
- [ ] No — questions are entirely user-defined and require high entropy  
- [ ] Yes — questions are fixed but include non-OSINT-discoverable options  
- [ ] Yes — questions are from a fixed list of common, OSINT-discoverable topics *(Vulnerable)*  

### Is rate limiting or account lockout applied to security question answer attempts?
- [ ] Yes — strict rate limiting and account lockout **are applied** to prevent brute-forcing  
- [ ] Yes — rate limiting **is applied** but bypass **is possible** via IP rotation or header manipulation  
- [ ] No — **no rate limiting** is enforced on answer attempts  

### Does the application enforce complexity or validation on the answer content?
- [ ] Yes — validation prevents common words and ensures answer complexity/uniqueness  
- [ ] Yes — basic validation is present but **can** be bypassed with common wordlists or dictionary attacks  
- [ ] No — **no validation** is performed; simple or single-character answers **are permitted**  

### Can the security question challenge be bypassed through logical flaws?
- [ ] No — the recovery flow strictly requires a valid answer to proceed  
- [ ] Yes — the recovery flow **can** be bypassed by manipulating HTTP response codes or session parameters  
- [ ] Yes — the answer is reflected in the source code or hidden fields of the page  

---