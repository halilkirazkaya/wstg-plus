## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Incubated vulnerabilities, also referred to as persistent or second-order vulnerabilities, occur when a malicious payload is stored by the application in a "cold" state and later executed or processed in a different context. This attack pattern typically targets back-end systems, administrative consoles, or automated reporting tools that retrieve data from a shared database or filesystem without adequate re-validation or output encoding. Pentesters must trace the lifecycle of user input from the point of entry (the "incubator") to every location where that data is eventually rendered or utilized by other components or secondary applications. Successful exploitation often leads to high-impact outcomes like administrative session hijacking via stored XSS or data exfiltration through second-order SQL injection in internal reporting modules.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Tools:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Are there endpoints where user-controlled input is stored for subsequent retrieval by other users or processes?
- [ ] No — application does **not** store user input for later use  
- [ ] Yes — input **is** stored in database fields, log files, or configuration settings  

### Is input validation or sanitization applied before data is written to the persistent storage layer?
- [ ] Yes — strict allow-list validation **is applied** at the point of entry  
- [ ] Yes — generic sanitization **is applied** but bypass **is possible** via encoding or non-standard characters  
- [ ] No — input is stored in its raw, unvalidated form  

### Is stored data properly encoded or sanitized when it is rendered in administrative or secondary interfaces?
- [ ] Yes — context-aware output encoding **is applied** everywhere the data is rendered  
- [ ] Yes — encoding **is applied** in some locations but **missing** in others (e.g., internal admin dashboard)  
- [ ] No — data is rendered without any encoding or sanitization, allowing for payload execution  

### Can payloads stored in the database affect back-end batch processes or out-of-band monitoring systems?
- [ ] No — back-end processes use safe parsing methods or parameterized queries  
- [ ] Yes — stored payloads **can** trigger interactions in back-end logic (e.g., CSV injection, command injection in logs)  
- [ ] Yes — stored payloads **can** trigger out-of-band (OOB) requests to attacker-controlled servers  

---