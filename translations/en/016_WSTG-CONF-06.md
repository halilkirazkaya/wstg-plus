## WSTG-CONF-06 — Test HTTP Methods

Testing HTTP methods involves identifying which verbs are supported by the web server and application to ensure only necessary functionality is exposed. Beyond standard `GET` and `POST`, servers may inadvertently enable dangerous methods like `PUT`, `DELETE`, or `TRACE`, which can allow attackers to upload malicious files, delete existing content, or exfiltrate session cookies via Cross-Site Tracing (XST). Pentesters examine response headers such as `Allow` or `Public` and attempt to bypass restrictions using method overriding techniques or verb tampering to access unauthorized functionality. This test is crucial for hardening the attack surface and preventing misconfigurations that could lead to server compromise or unauthorized data manipulation.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity becomes High if `PUT` or `DELETE` allow unauthorized file manipulation or if `TRACE` facilitates session token exfiltration.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Are dangerous HTTP methods like `PUT` or `DELETE` disabled across the application?
- [ ] Yes — only safe methods (GET, POST, HEAD) **are enabled**  
- [ ] No — `PUT` or `DELETE` **are enabled** but require valid authentication  
- [ ] No — `PUT` or `DELETE` **are enabled** and accessible without authentication *(Critical)*  

### Is the `TRACE` method disabled to prevent Cross-Site Tracing (XST)?
- [ ] Yes — `TRACE` method **is disabled** or returns a 405 Method Not Allowed  
- [ ] No — `TRACE` method **is enabled** but does not reflect sensitive headers  
- [ ] No — `TRACE` method **is enabled** and reflects `Cookie` or `Authorization` headers *(Medium)*  

### Is HTTP Method Overriding (e.g., `X-HTTP-Method-Override`) supported by the server?
- [ ] No — server does **not** process method override headers  
- [ ] Yes — server processes override headers but access controls **are enforced**  
- [ ] Yes — server processes override headers and access control bypass **is possible**  

### Does the server respond securely to arbitrary or malformed HTTP methods?
- [ ] Yes — server returns a 405 Method Not Allowed or 501 Not Implemented  
- [ ] No — server returns a 200 OK or 500 Error for arbitrary methods like `JEFF` or `TEST`  
- [ ] No — using arbitrary methods allows bypassing authentication or authorization filters  

### Is the `OPTIONS` method restricted or configured to minimize information disclosure?
- [ ] Yes — `OPTIONS` is disabled or returns a minimal `Allow` header  
- [ ] No — `OPTIONS` reveals a wide range of enabled methods, including DAV or debugging verbs  

---