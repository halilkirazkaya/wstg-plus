## WSTG-SESS-04 — Testing for Exposed Session Variables

Exposed session variables occur when sensitive session identifiers or state-related data are transmitted via insecure channels such as URL query strings, Referer headers, or system logs. This exposure significantly increases the risk of session hijacking, as identifiers can be cached in browser history, logged by intermediate proxies, or leaked to third-party sites via the Referer header. Pentesters identify these exposures by monitoring all outbound requests and inspecting application logs or server configurations for inadvertent data leakage. In the real world, attackers exploit these leaks by harvesting valid session tokens from shared machines or analyzing traffic patterns to bypass authentication and gain unauthorized access to user accounts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the exposed variable is a session token that allows immediate account takeover.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Is the session identifier transmitted in the URL query string?
- [ ] No — session identifiers are **not** present in the URL query string  
- [ ] Yes — identifiers are present but session is short-lived or low-risk  
- [ ] Yes — unique session IDs **are** present in the URL query string *(Critical)*  

### Does the application leak session tokens through the Referer header?
- [ ] No — `Referrer-Policy` prevents leakage or no third-party links exist  
- [ ] Yes — session tokens **are** transmitted to third-party domains via the Referer header  

### Are session variables stored in server-side or application logs?
- [ ] No — session variables are masked or excluded from logs  
- [ ] Yes — session variables **are** recorded in application or web server logs in plain text  

### Does the application use GET requests for state-changing operations?
- [ ] No — application uses `POST` or other non-idempotent methods for sensitive operations  
- [ ] Yes — `GET` is used, causing session data to be cached in browser history and intermediate proxies  

### Is the `Cache-Control` header configured to prevent caching of session-related data?
- [ ] Yes — `Cache-Control: no-store` (or similar) **is applied** to sensitive responses  
- [ ] Yes — controls are in place but bypass **is possible** via browser edge cases  
- [ ] No — sensitive responses containing session data **can** be cached by the browser  

---