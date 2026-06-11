## WSTG-CONF-13 — Path Confusion

Path Confusion arises from discrepancies in how different web components, such as reverse proxies, load balancers, and backend application servers, parse and interpret URL paths. Attackers exploit these inconsistencies by injecting specific characters like semicolons, encoded slashes, or dot-segments to deceive security filters and access restricted endpoints or static resources. This vulnerability can lead to critical security failures, including authentication bypass, unauthorized data access, and web cache poisoning, as the front-end may apply security rules to a path that the back-end interprets differently. Successful exploitation typically occurs at the architectural boundary where request routing and normalization logic diverge across the tech stack.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if path confusion results in a bypass of authentication or access control for sensitive administrative endpoints.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Do different architectural layers interpret path delimiters (e.g., `;`, `#`, `?`) consistently?
- [ ] Yes — all layers normalize and interpret path delimiters identically  
- [ ] No — inconsistencies exist but **cannot** be used to access restricted resources  
- [ ] No — discrepancies allow an attacker to bypass front-end filters and reach back-end logic **is possible**  

### Can access controls be bypassed using path traversal sequences or encoded characters?
- [ ] No — normalization is applied consistently before security checks  
- [ ] Yes — bypass is **possible** via dot-segment sequences (e.g., `/admin/..;/`)  
- [ ] Yes — bypass is **possible** via URL-encoded characters (e.g., `%2f`, `%2e%2e%2f`)  

### Is the application susceptible to Web Cache Poisoning through path confusion?
- [ ] No — caching logic is not affected by path ambiguity or extra path info  
- [ ] Yes — malicious content **can** be cached for legitimate paths due to mapping discrepancies  
- [ ] Yes — sensitive information **can** be cached in public directories via `RCD` (Relative Path Overwrite) techniques  

### Does the server handle "extra path information" (PathInfo) securely?
- [ ] No — feature is **disabled** or does not exist  
- [ ] Yes — `PathInfo` is handled and does **not** interfere with security filters  
- [ ] No — `PathInfo` allows an attacker to append data that confuses the application's routing logic  

---