## WSTG-CONF-09 — Test File Permission

Testing file permissions involves auditing the access control levels assigned to files and directories on the web server to ensure that sensitive resources are not over-exposed to unauthorized users or processes. Improperly configured permissions can allow attackers to read sensitive configuration files containing database credentials, view proprietary source code, or modify server-side scripts to achieve remote code execution. This vulnerability typically occurs during the deployment phase when default "world-readable" or "world-writable" flags are left on critical application directories, such as configuration paths, log folders, or upload repositories. From an attacker's perspective, discovering an improperly secured file often serves as the primary catalyst for privilege escalation or full system compromise.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if sensitive configuration files (e.g., `.env`, `web.config`) are readable or if write access to the web root is possible.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Tools:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Are sensitive application configuration files protected from unauthorized access?
- [ ] Yes — configuration files (e.g., `.env`, `config.php`) are **not accessible** via web requests  
- [ ] Yes — files are present but access is **restricted** to authorized local users only  
- [ ] No — sensitive configuration files **are accessible** and disclose credentials or secrets  

### Can an attacker modify files or directories within the web root?
- [ ] No — all web root directories are **read-only** for the web server user  
- [ ] Yes — writable directories exist but script execution is **disabled**  
- [ ] Yes — world-writable directories exist and **allow** for the upload and execution of scripts *(Critical)*  

### Are version control metadata or backup files exposed due to lax permissions?
- [ ] No — `.git`, `.svn`, and backup files (e.g., `.bak`, `~`) are **not present** or protected  
- [ ] Yes — metadata directories exist but directory listing is **disabled**  
- [ ] Yes — sensitive metadata or backups are **fully accessible**, allowing for source code recovery  

### Does the web server user operate with the principle of least privilege?
- [ ] Yes — the web server process runs as a **dedicated low-privilege** user  
- [ ] No — the web server process runs with **unnecessary privileges** (e.g., `root` or `Administrator`)  

### Are log files secured against unauthorized reading or modification?
- [ ] Yes — log files are **restricted** to administrative users  
- [ ] Yes — log files are readable but do **not** contain session tokens or PII  
- [ ] No — log files are **publicly readable** and disclose sensitive transaction data or user info  

---