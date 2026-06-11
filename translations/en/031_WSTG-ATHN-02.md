## WSTG-ATHN-02 — Testing for Default Credentials

Testing for default credentials involves identifying administrative interfaces, services, or application components that still use factory-set or vendor-provided usernames and passwords. This vulnerability is a high-priority target for attackers because it often provides an immediate path to full system compromise, administrative access, or remote code execution with minimal effort. It typically occurs in off-the-shelf software, CMS platforms, database management tools, and integrated hardware like IoT devices or network appliances. Exploitation is usually performed by cross-referencing identified software versions against public default password databases or using automated credential stuffing tools during the initial reconnaissance and exploitation phases.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Test Status** | Not Performed |
| **Severity** | Critical / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Are administrative or management interfaces exposed to the internet or untrusted network segments?
- [ ] No — no management interfaces are accessible  
- [ ] Yes — interfaces are present but restricted by network-level controls (e.g., VPN, IP Allowlist)  
- [ ] Yes — interfaces **are** publicly accessible and require authentication  

### Have vendor-supplied default credentials been changed for all identified components?
- [ ] Yes — all default credentials **have been changed** across all components *(Most secure)*  
- [ ] Yes — credentials **have been changed** for the main application, but secondary components (e.g., CMS, DB) remain default  
- [ ] No — default credentials **are active** on one or more identifiable interfaces *(Critical)*  

### Does the application enforce a password change upon first login for new or administrative accounts?
- [ ] Yes — a password change **is mandatory** before any administrative action can be taken  
- [ ] No — the application **allows** the use of default credentials indefinitely  

### Are lockout mechanisms or rate-limiting controls active to prevent automated default credential testing?
- [ ] Yes — aggressive rate limiting or account lockout **is applied**  
- [ ] Yes — controls are present but bypass **is possible** via header manipulation or IP rotation  
- [ ] No — no rate limiting or lockout mechanisms **are enabled**  

### Do third-party plugins, middleware, or administrative tools (e.g., phpMyAdmin, Tomcat Manager) use unique credentials?
- [ ] No — third-party components do not exist in the environment  
- [ ] Yes — all third-party components use unique, non-default credentials  
- [ ] No — at least one third-party component **is accessible** via known default credentials  

---