## WSTG-ATHN-05 — Testing for Vulnerable Remember Password

The "Remember Me" or persistent login functionality is designed to maintain a user's authenticated state across browser restarts by storing a token or credential on the client side. If this feature is poorly implemented—such as by storing plaintext passwords, weak hashes, or low-entropy tokens in cookies—it exposes the user to account takeover through Cross-Site Scripting (XSS), physical access, or network interception. Pentesters evaluate the entropy of these persistence tokens, the security flags applied to the storage mechanism, and the lifecycle of the session to ensure that the convenience of persistent access does not bypass critical security controls like multi-factor authentication or session invalidation. Exploitation often involves harvesting these long-lived tokens to gain unauthorized access to accounts without requiring the original credentials.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if plaintext credentials or unsalted hashes are stored in the persistent cookie, or if the token bypasses Multi-Factor Authentication (MFA).

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Does the application implement a "Remember Me" or "Stay Logged In" feature?
- [ ] No — feature does **not** exist in the application  
- [ ] Yes — feature exists and uses cryptographically strong, non-predictable tokens  
- [ ] Yes — feature exists but uses predictable or low-entropy tokens *(Medium)*  
- [ ] Yes — feature exists and stores plaintext credentials or base64-encoded passwords *(High)*  

### Are the security flags correctly applied to the persistent cookie?
- [ ] Yes — `HttpOnly`, `Secure`, and `SameSite` flags are **applied**  
- [ ] Yes — some flags are applied but `HttpOnly` is **missing**  
- [ ] Yes — some flags are applied but `Secure` is **missing** over HTTPS  
- [ ] No — no security flags are **applied** to the persistent cookie  

### Does the "Remember Me" token remain valid after a manual logout?
- [ ] No — token is invalidated on the server-side immediately upon logout  
- [ ] Yes — token remains valid but requires a password for sensitive actions  
- [ ] Yes — token remains valid and allows full account access **without** re-authentication *(Medium)*  

### Is the persistent session terminated upon a password change?
- [ ] Yes — all persistent tokens are revoked when the user changes their password  
- [ ] No — the "Remember Me" token remains valid after a password reset/change **is performed** *(High)*  

### Does the persistent token bypass Multi-Factor Authentication (MFA) on subsequent visits?
- [ ] No — MFA is required even when a "Remember Me" token is present  
- [ ] Yes — MFA is bypassed, but the token is tied to a specific device fingerprint  
- [ ] Yes — MFA is **fully bypassed** using only the persistent cookie from any device *(High)*  

---