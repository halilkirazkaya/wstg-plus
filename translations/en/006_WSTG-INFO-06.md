## WSTG-INFO-06 — Identify Application Entry Points

Identifying application entry points involves mapping the entire attack surface by enumerating all possible ways a user or external system can interact with the application. This process includes discovering all URLs, API endpoints, parameters (GET, POST, cookie-based), HTTP headers, and specialized protocols like WebSockets or gRPC. For a penetration tester, this stage is vital because vulnerabilities are often found in undocumented "shadow" APIs, legacy endpoints, or complex parameter structures that were overlooked during development. Thorough enumeration ensures that no component of the application remains an untested black box, thereby preventing attackers from finding unmonitored routes into the system.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Test Status** | Not Performed |
| **Severity** | Informational |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Have all GET and POST parameters been enumerated across the application?
- [ ] Yes — comprehensive enumeration via automated and manual crawling **is complete**  
- [ ] Yes — only common parameters have been identified via standard crawling  
- [ ] No — parameters are largely **unidentified** or untested  

### Are undocumented or hidden parameters (e.g., debug, admin) searched for using fuzzing?
- [ ] No — hidden parameter discovery is **not necessary** for this specific scope  
- [ ] Yes — parameter fuzzing (e.g., using Arjun) **has been performed** with no findings  
- [ ] Yes — hidden parameters **were discovered** and mapped for further testing  
- [ ] No — hidden parameter discovery **was not** performed  

### Have all supported HTTP methods (PUT, DELETE, PATCH, etc.) been identified for each endpoint?
- [ ] Yes — method discovery **is applied** across all discovered endpoints  
- [ ] Yes — method discovery **is applied** only to high-interest or sensitive endpoints  
- [ ] No — only standard GET and POST methods **are identified**  

### Are non-standard entry points like WebSockets, gRPC, or custom headers mapped?
- [ ] No — application **does not** utilize non-standard protocols or custom headers  
- [ ] Yes — all protocol-specific entry points and custom headers **are identified**  
- [ ] No — non-standard entry points **exist** but have not been mapped  

### Has the API surface (including versioned endpoints like /v1/ or /v2/) been fully mapped?
- [ ] No — application **does not** expose an API  
- [ ] Yes — all API versions and endpoints **are identified** and documented  
- [ ] Yes — only the current production API version **is identified**  
- [ ] No — API endpoints **remain unmapped** or only partially discovered  

---