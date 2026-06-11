## WSTG-ATHN-04 — Testing for Bypassing Authentication Schema

Bypassing the authentication schema involves identifying and exploiting flaws in the application logic that allow an attacker to gain access to protected resources without providing valid credentials. This vulnerability typically occurs when the application relies on client-side controls, fails to enforce server-side authorization on every request, or leaves administrative "backdoors" and debug endpoints exposed in production. Attackers exploit these weaknesses by performing forced browsing to sensitive URLs, manipulating HTTP parameters like `authenticated=true`, or spoofing headers to trick the application into assuming a session is already established. Successful exploitation results in a complete breakdown of the authentication perimeter, potentially leading to unauthorized data exfiltration or full system compromise.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Can sensitive internal pages be accessed directly without an active session?
- [ ] No — direct access results in redirection to login or a 403/404 response  
- [ ] Yes — some sensitive pages are accessible but provide limited or non-sensitive data  
- [ ] Yes — sensitive administrative or user-specific pages **are** fully accessible without authentication *(Critical)*  

### Does the application rely on client-side flags or parameters to determine authentication status?
- [ ] No — authentication status is managed strictly via server-side session state  
- [ ] Yes — client-side flags exist but modification **does not** grant access to protected areas  
- [ ] Yes — modifying parameters (e.g., `is_authenticated=true`, `user_role=admin`) **allows** authentication bypass  

### Can authentication be bypassed by spoofing or manipulating specific HTTP headers?
- [ ] No — headers such as `X-Forwarded-For`, `Referer`, or custom headers have no impact on authentication  
- [ ] Yes — manipulating headers (e.g., `X-Custom-IP-Authorization`, `X-Remote-User`) **is possible** and grants unauthorized access  

### Are there alternative paths or development artifacts that circumvent the standard login flow?
- [ ] No — all protected resources are gated by a centralized, production-ready authentication check  
- [ ] Yes — legacy endpoints, debug routes (e.g., `/debug/login`), or forgotten API versions **are** accessible without credentials  

### Does the application fail to re-authenticate or verify sessions for high-privilege actions?
- [ ] No — high-privilege or sensitive actions require a fresh or valid session token  
- [ ] Yes — once a bypass is found on one endpoint, it **can** be used to perform administrative actions across the application  

---