## WSTG-CONF-03 — Test File Extensions Handling for Sensitive Information

Testing for file extension handling involves identifying whether the web server or application server reveals sensitive information by serving files that should be restricted or executed. Attackers frequently probe for backup files, configuration files, and source code fragments (e.g., `.bak`, `.old`, `.env`, `.inc`) that may be left in the web root and served as plain text due to a lack of specific handler mappings. This vulnerability matters because it can lead to the exposure of database credentials, API keys, and internal business logic, facilitating deeper exploitation of the infrastructure. These exposures typically occur in public directories, upload folders, or via misconfigured MIME-type settings on the server.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if configuration files containing credentials or full source code are accessible.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Are sensitive backup or temporary file extensions (e.g., `.bak`, `.old`, `.swp`, `~`) accessible?
- [ ] No — server returns 403 Forbidden or 404 Not Found for common backup extensions  
- [ ] Yes — server allows downloading of backup files but they **do not** contain sensitive data  
- [ ] Yes — server allows downloading of backup files containing sensitive data or source code *(High)*  

### Does the server expose environment or system configuration files (e.g., `.env`, `.config`, `.yml`, `.ini`)?
- [ ] No — restricted configuration files **cannot** be accessed and return 403/404  
- [ ] Yes — sensitive configuration files **are** accessible and disclose internal secrets or credentials  

### Can source code be retrieved by appending alternate extensions (e.g., `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] No — application code is **not** rendered as plain text regardless of extension manipulation  
- [ ] Yes — source code is revealed because the server **does not** have a handler for the appended extension  

### Are administrative or metadata directories (e.g., `.git/`, `.svn/`, `.DS_Store`) blocked from public access?
- [ ] No — these directories/files do **not** exist in the web root  
- [ ] Yes — access is **not possible** due to server-side rewrite rules or permissions  
- [ ] Yes — metadata directories **are** accessible and allow for full source code reconstruction  

### How does the server handle requests for files with multiple extensions (e.g., `file.php.jpg`)?
- [ ] No — the server correctly prioritizes the security of the internal extension or blocks the request  
- [ ] Yes — the server processes the file as the first extension, but bypass **is not possible** for execution  
- [ ] Yes — the server ignores the final extension, allowing code execution or source disclosure **is possible**  

---