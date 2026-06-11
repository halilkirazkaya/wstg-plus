## WSTG-CONF-04 — Review Old Backup and Unreferenced Files for Sensitive Information

Reviewing old backup and unreferenced files involves identifying forgotten, temporary, or hidden files on a web server that are not intended for public access. These files often include source code backups (`.zip`, `.bak`), text editor swap files (`.swp`, `~`), or source control metadata (`.git`, `.svn`) that can leak sensitive information. Attackers use automated wordlists and discovery tools to brute-force common naming conventions, aiming to exfiltrate database credentials, hardcoded API keys, or logic that facilitates further exploitation. This vulnerability typically stems from manual server maintenance or flawed CI/CD pipelines that fail to sanitize the production web root.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if configuration files, source code archives, or credentials are discovered.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Are directory listings enabled on the web server?
- [ ] No — directory listing is **disabled** and returns a 403 Forbidden or custom error  
- [ ] Yes — directory listing is **enabled** on non-sensitive directories  
- [ ] Yes — directory listing is **enabled** on directories containing source code or sensitive files *(Critical)*  

### Are backup files with common extensions (e.g., .bak, .old, .save) discoverable?
- [ ] No — common backup extensions are **not found** or blocked by server configuration  
- [ ] Yes — backup files exist but do **not** contain sensitive information  
- [ ] Yes — backup files for configuration or source code **are** accessible *(High)*  

### Are source control directories (e.g., .git, .svn) or metadata exposed?
- [ ] No — source control metadata is **not present** or correctly blocked  
- [ ] Yes — metadata exists but the full repository **cannot** be reconstructed  
- [ ] Yes — the entire source code repository **can** be exfiltrated via exposed metadata  

### Are compressed archives (e.g., .zip, .tar.gz, .rar) of the application present in the web root?
- [ ] No — no sensitive archive files discovered via brute-force or enumeration  
- [ ] Yes — archives found but they are password protected or contain public assets  
- [ ] Yes — unencrypted archives containing application source or data **are** publicly accessible  

### Does the application leak temporary files created by text editors or IDEs?
- [ ] No — temporary files like `.swp`, `~`, or `.DS_Store` are **not present**  
- [ ] Yes — non-sensitive temporary files are **present**  
- [ ] Yes — temporary files reveal snippets of source code or internal paths  

---