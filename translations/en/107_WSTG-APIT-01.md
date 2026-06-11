## WSTG-APIT-01 — API Reconnaissance

API reconnaissance is the systematic process of identifying and mapping the API attack surface to uncover endpoints, supported methods, and underlying data structures. Attackers perform this phase to discover undocumented (shadow) APIs, deprecated versions with legacy vulnerabilities, and exposed documentation files like Swagger or OpenAPI definitions that reveal internal business logic. By analyzing predictable URL patterns, client-side JavaScript files, and directory brute-force results, an adversary can build a comprehensive map of the API to identify targets for Broken Object Level Authorization (BOLA) or Mass Assignment attacks. This reconnaissance typically targets common paths such as `/api/v1/`, `/swagger.json`, or `/graphql` to gain a roadmap of the application's backend functionality.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational / Low |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Tools:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Are API documentation files (Swagger, OpenAPI, WSDL) publicly accessible?
- [ ] No — API documentation is **not** accessible or is restricted by authentication  
- [ ] Yes — documentation is accessible but requires valid credentials  
- [ ] Yes — API documentation (e.g., `/swagger-ui.html`) is **publicly accessible** without authentication  

### Can undocumented or "shadow" API endpoints be discovered via fuzzing?
- [ ] No — directory and endpoint fuzzing yielded no undocumented paths  
- [ ] Yes — undocumented endpoints were found but they **cannot** be accessed without authorization  
- [ ] Yes — undocumented endpoints were found and **can** be accessed without valid authorization  

### Does the application reveal multiple API versions (e.g., /v1/, /v2/, /beta/)?
- [ ] No — only the current, hardened API version is accessible  
- [ ] Yes — legacy versions exist but implement the **same** security controls as the current version  
- [ ] Yes — legacy versions (e.g., `/v1/`) are accessible and **lack** the security controls of newer versions  

### Do client-side resources (JavaScript/Mobile Apps) leak API endpoint structures or keys?
- [ ] No — client-side code does **not** contain hardcoded API paths or sensitive keys  
- [ ] Yes — client-side code contains API endpoint maps but **no** sensitive keys  
- [ ] Yes — API keys, secrets, or internal-only endpoint URLs **are** hardcoded in client-side resources  

### Are hidden parameters or headers discoverable on known endpoints?
- [ ] No — parameter fuzzing (e.g., via Arjun) did **not** reveal hidden inputs  
- [ ] Yes — hidden parameters (e.g., `debug=true`, `admin=1`) were discovered but are **not** functional  
- [ ] Yes — hidden parameters were discovered and **can** be used to alter application behavior  

---