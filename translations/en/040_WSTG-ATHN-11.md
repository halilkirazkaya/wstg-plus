## WSTG-ATHN-11 — Testing Multi-Factor Authentication (MFA)

Multi-factor authentication (MFA) testing evaluates the robustness of the secondary security layer designed to prevent unauthorized access even when primary credentials are compromised. Attackers frequently attempt to bypass MFA by identifying endpoints where it is inconsistently enforced, such as legacy API versions, mobile backends, or password reset workflows. Exploitation often involves manipulating server responses, brute-forcing short-lived codes (OTP), or exploiting race conditions and session management flaws that allow a user to transition to an authenticated state without providing the second factor.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Is MFA enforced consistently across all authentication portals?
- [ ] Yes — MFA is required for all web, mobile, and API-based login attempts  
- [ ] Yes — but MFA is **not enforced** on specific endpoints (e.g., legacy `/api/v1/login` or mobile-specific portals)  
- [ ] No — MFA is **not implemented** in the application *(Critical)*  

### Can the MFA verification step be bypassed via direct URL navigation or response manipulation?
- [ ] No — direct navigation or parameter tampering **cannot** bypass the challenge  
- [ ] Yes — navigating directly to internal dashboard URLs **is possible** without completing the MFA challenge  
- [ ] Yes — manipulating the server response (e.g., changing a `401 Unauthorized` to `200 OK`) **is possible** to gain access  

### Is the MFA code verification process protected against brute-force attacks?
- [ ] Yes — strict rate limiting or account lockout **is applied** after multiple failed OTP attempts  
- [ ] Yes — rate limiting exists but **is possible** to bypass via IP rotation or header manipulation  
- [ ] No — no rate limiting is enforced, and automated brute-forcing of codes **is possible**  

### Does the application maintain a secure session state during the MFA transition?
- [ ] Yes — high-privileged session tokens are issued **only after** successful MFA completion  
- [ ] No — a fully functional session cookie is issued **before** MFA is completed, allowing access to some features  
- [ ] No — the application uses a static identifier or predictable session transition that **can** be hijacked  

### Are alternative MFA factors (SMS, Email, Backup Codes) vulnerable to exploitation?
- [ ] No — all factors use secure, non-predictable, and time-limited values  
- [ ] Yes — backup codes are predictable or **can** be enumerated  
- [ ] Yes — the "Resend Code" functionality can be abused to perform SMS/Email flooding or reveal partial contact details  

---