## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) is a vulnerability where an attacker trick a victim's browser into performing an unwanted action on a different website where the victim is currently authenticated. This exploit leverages the browser's behavior of automatically attaching "ambient" credentials, such as session cookies or Authorization headers, to outgoing requests. Attackers typically target state-changing operations like changing passwords, updating email addresses, or executing financial transfers by hosting malicious scripts or hidden forms on a third-party site. Successful exploitation can lead to full account takeover or unauthorized data modification without the user's knowledge or consent.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the vulnerable action allows for account takeover, privilege escalation, or unauthorized financial transactions.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Tools:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Are anti-CSRF tokens implemented for state-changing requests?
- [ ] Yes — unique, cryptographically strong tokens are required for all state-changing actions  
- [ ] Yes — tokens are present but are **not** unique per session or are predictable  
- [ ] No — anti-CSRF tokens are **not** implemented  

### Is the server-side validation of the anti-CSRF token robust?
- [ ] Yes — server strictly validates the token and bypass **not possible**  
- [ ] Yes — validation is performed but **can** be bypassed by removing the token parameter  
- [ ] Yes — validation is performed but **can** be bypassed by providing a blank or dummy token  
- [ ] Yes — validation is performed but token is **not** tied to the user's session  

### Does the application rely on easily bypassable methods for CSRF protection?
- [ ] No — application does not rely on weak headers or origin checks alone  
- [ ] Yes — protection relies solely on the `Referer` or `Origin` header which **can** be spoofed or stripped  
- [ ] Yes — protection relies on checking the `X-Requested-With` header which **can** be bypassed via CORS misconfigurations  

### Are session cookies configured with the `SameSite` attribute?
- [ ] Yes — `SameSite` is set to `Strict` or `Lax` on all session-related cookies  
- [ ] No — `SameSite` attribute is **missing**, defaulting to browser-specific behavior  
- [ ] No — `SameSite` is explicitly set to `None` without the `Secure` flag or additional mitigations  

### Can the CSRF protection be bypassed by changing the HTTP request method?
- [ ] No — protection is enforced regardless of the HTTP method used  
- [ ] Yes — tokens are only validated on `POST` requests, but the action **can** be performed via `GET`  
- [ ] Yes — changing the method (e.g., to `PUT` or `DELETE`) bypasses the token check  

---