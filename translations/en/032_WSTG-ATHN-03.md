## WSTG-ATHN-03 — Testing for Weak Lock Out Mechanism

An account lockout mechanism is a critical defense-in-depth control designed to mitigate brute-force and credential stuffing attacks by disabling an account after a specified number of failed authentication attempts. Without a robust lockout policy, attackers can use automated tools to systematically guess passwords across multiple accounts (password spraying) or target a single high-value account until successful. This vulnerability typically manifests on login portals, password reset forms, and API endpoints that lack rate limiting or account-level protection. From an attacker's perspective, a weak or absent lockout mechanism allows for infinite password attempts, while an overly aggressive one can be exploited to cause a distributed Denial of Service (DoS) against legitimate users by intentionally locking their accounts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the application handles sensitive PII, financial data, or if administrative accounts are susceptible to brute-force without any secondary controls.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Is an account lockout mechanism enforced after a set number of failed attempts?
- [ ] Yes — account is locked after a defined, reasonable threshold (e.g., 5-10 attempts)  
- [ ] Yes — account is locked but only after an excessively high number of attempts  
- [ ] No — account **cannot** be locked and allows for indefinite brute-force  

### Can the lockout mechanism be bypassed using common evasion techniques?
- [ ] No — lockout is enforced server-side and is **not** affected by client-side changes  
- [ ] Yes — lockout **is possible** to bypass by rotating IP addresses or using a proxy pool  
- [ ] Yes — lockout **is possible** to bypass by manipulating HTTP headers like `X-Forwarded-For` or `User-Agent`  
- [ ] Yes — lockout **is possible** to bypass by clearing cookies or starting new sessions  

### Does the lockout mechanism provide a path for account recovery or expiration?
- [ ] Yes — account is automatically unlocked after a set duration or via email verification  
- [ ] Yes — account requires administrative intervention to unlock  
- [ ] No — lockout is permanent with no clear recovery path, increasing DoS risk  

### Does the application differentiate between a valid and invalid username during lockout?
- [ ] No — the application response is identical for both existing and non-existing users  
- [ ] Yes — the application reveals if an account is locked, allowing for **username enumeration**  

### Is the lockout mechanism susceptible to a Denial of Service (DoS) attack?
- [ ] No — controls like CAPTCHA or IP-based rate limiting prevent mass account locking  
- [ ] Yes — an attacker **can** systematically lock out all known users by providing incorrect passwords  

---