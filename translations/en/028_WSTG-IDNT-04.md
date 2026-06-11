## WSTG-IDNT-04 — Testing for Account Enumeration and Guessable User Account

Account enumeration occurs when an application reveals the existence or non-existence of a user account through variations in HTTP responses, error messages, or measurable timing differences. Attackers exploit this behavior by systematically submitting lists of usernames to identify valid accounts, which are subsequently targeted for credential stuffing, brute-force attacks, or social engineering. This vulnerability commonly manifests in login portals, password reset features, registration forms, and API endpoints that handle user-specific identifiers. From an attacker's perspective, successful enumeration significantly narrows the attack surface by transforming a blind brute-force attempt into a targeted exploitation of known, valid identities.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Are authentication error messages consistent across all scenarios?
- [ ] Yes — generic error messages are used for both invalid usernames and invalid passwords  
- [ ] Yes — messages are similar, but slight variations in punctuation or casing **are present**  
- [ ] No — error messages explicitly state "User not found" or "Incorrect password" *(Vulnerable)*  

### Does the password reset or "forgot password" flow leak account existence?
- [ ] No — the application returns a generic message regardless of whether the email/username exists  
- [ ] Yes — controls are in place, but bypass **is possible** via response length or status codes  
- [ ] Yes — the application explicitly confirms if a reset email was sent or if the account **does not exist**  

### Is the registration process protected against user enumeration?
- [ ] No — registration does not leak information or uses CAPTCHA/rate-limiting to prevent bulk checks  
- [ ] Yes — controls are in place, but bypass **is possible** via timing or API response differences  
- [ ] Yes — the application immediately notifies the user if the username or email **is already taken**  

### Are timing differences measurable when comparing valid vs. invalid usernames?
- [ ] No — response times are consistent regardless of account validity due to constant-time comparisons  
- [ ] Yes — timing differences exist but are too small to be reliably measured over the network  
- [ ] Yes — measurable timing discrepancies **are present** (e.g., due to password hashing execution only for valid users)  

### Does the application use predictable or guessable naming conventions for accounts?
- [ ] No — account identifiers are random (UUIDs) or user-defined and complex  
- [ ] Yes — account identifiers follow a pattern (e.g., `emp001`, `emp002`) and enumeration **is possible**  
- [ ] Yes — identifiers are sequential integers, making full database enumeration **possible**  

---