## WSTG-CONF-07 — Test HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) is a security mechanism that allows a web server to inform browsers that it should only be accessed via HTTPS, preventing protocol downgrade attacks and cookie hijacking. By enforcing an encrypted connection, HSTS mitigates Man-in-the-Middle (MitM) attacks such as SSLStrip, where an attacker intercepts an initial insecure HTTP request to prevent the transition to TLS. From an attacker's perspective, the absence of this header or a short `max-age` duration provides a window of opportunity to intercept sensitive session tokens or credentials during the initial cleartext handshake. This test focuses on verifying the presence, duration, and scope of the `Strict-Transport-Security` header across all sensitive application endpoints.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity becomes Medium if the application handles PII, session tokens, or credentials, as the lack of HSTS increases the success rate of MitM attacks.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Is the `Strict-Transport-Security` header present in HTTPS responses?
- [ ] Yes — header **is present** on all sensitive endpoints  
- [ ] Yes — header **is present** but only on the landing page  
- [ ] No — header **is missing** from the application entirely  

### Is the `max-age` directive set to a sufficient duration?
- [ ] Yes — `max-age` is set to one year or more (e.g., `31536000`)  
- [ ] Yes — `max-age` is set but is **too short** (e.g., less than 6 months)  
- [ ] No — `max-age` directive **is missing** or set to `0` *(Critical)*  

### Does the HSTS policy cover all subdomains?
- [ ] Yes — `includeSubDomains` directive **is applied**  
- [ ] No — `includeSubDomains` directive **is missing**, leaving subdomains vulnerable to downgrade  
- [ ] No — feature does not apply as no subdomains exist  

### Is the application registered for HSTS Preloading?
- [ ] Yes — `preload` directive **is present** and domain is in the HSTS preload list  
- [ ] Yes — `preload` directive **is present** but domain is **not** yet in the preload list  
- [ ] No — `preload` directive **is missing**, allowing a window for MitM on the first visit  

### Is the HSTS header incorrectly sent over plaintext HTTP?
- [ ] No — header is **only** sent over secure HTTPS connections  
- [ ] Yes — header **is sent** over HTTP, which is ignored by browsers and may leak configuration details  

---