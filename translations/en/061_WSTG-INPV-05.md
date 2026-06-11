## WSTG-INPV-05 — SQL Injection

SQL Injection occurs when user-supplied input is incorporated into SQL queries without proper parameterization or sanitization, allowing attackers to manipulate query logic. Successful exploitation can lead to unauthorized data exfiltration, authentication bypass, data modification, and in some cases remote code execution via database features such as `xp_cmdshell` or `LOAD_FILE()`. This vulnerability commonly affects login forms, search functionality, REST API parameters, HTTP headers, and any endpoint that constructs dynamic SQL queries from user-controlled input. From an attacker's perspective, identifying an injection point is the primary gateway to full database compromise and potential lateral movement within the infrastructure.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Tools:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Are user-controllable parameters processed using secure database access methods?
- [ ] Yes — application uses ORM or parameterized queries exclusively and bypass **not possible**  
- [ ] Yes — parameterized queries are used but bypass **is possible** via edge cases (e.g., `ORDER BY` clauses)  
- [ ] No — raw string concatenation is used to build SQL queries **without** parameterization  

### Can the authentication mechanism be circumvented via SQL injection?
- [ ] No — login parameters are **not** vulnerable to injection  
- [ ] Yes — authentication bypass **is possible** using tautology-based payloads (e.g., `' OR 1=1 --`)  

### Is data exfiltration possible via in-band or out-of-band techniques?
- [ ] No — no data exfiltration identified  
- [ ] Yes — UNION-based or error-based exfiltration **is possible**  
- [ ] Yes — blind (Boolean or Time-based) exfiltration **is possible**  
- [ ] Yes — out-of-band (DNS/HTTP) exfiltration **is possible**  

### Do application functions exhibit second-order SQL injection vulnerabilities?
- [ ] No — stored data is safely handled when retrieved for later queries  
- [ ] Yes — user input is stored and later used in an unsafe SQL query **without** validation  

### Are database service account privileges restricted to the minimum required?
- [ ] Yes — service account has **least privilege** (e.g., limited to specific tables/actions)  
- [ ] No — service account has elevated privileges (e.g., `DROP TABLE`, `FILE` privileges, or `xp_cmdshell` **enabled**)  

---