## WSTG-INFO-01 — Conduct Search Engine Discovery and Reconnaissance for Information Leakage

Search engine discovery involves leveraging public search engines, cached pages, and indexing services to identify information that the target organization has unintentionally exposed. Attackers use advanced search operators (Google Dorks) and third-party indexing services to discover sensitive files, directories, login portals, error messages, and metadata that should not be publicly accessible. This reconnaissance phase is critical because it requires zero direct interaction with the target, making it virtually undetectable while potentially revealing high-value entry points such as exposed admin panels, configuration files, database dumps, and internal documentation.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Are sensitive files or directories indexed by search engines?
- [ ] No — no sensitive content found in search engine results  
- [ ] Yes — configuration files, backups, or internal documents **are** indexed *(Critical)*  

### Does Google Dorking reveal exposed administrative or login interfaces?
- [ ] No — admin panels and login pages are **not** indexed  
- [ ] Yes — admin panels **are** indexed but require strong multi-factor authentication  
- [ ] Yes — admin panels **are** indexed and publicly accessible **without** sufficient controls  

### Are cached versions of pages exposing sensitive data that has since been removed?
- [ ] No — no sensitive data found in cached snapshots  
- [ ] Yes — cached pages contain credentials, internal IP addresses, or sensitive information  

### Is the `robots.txt` file unintentionally disclosing sensitive paths?
- [ ] No — `robots.txt` does **not** reveal sensitive directory structures  
- [ ] Yes — `robots.txt` lists sensitive paths that aid attacker reconnaissance  
- [ ] No `robots.txt` file exists  

### Are third-party services (Shodan, Censys, Wayback Machine) revealing historical or current exposure?
- [ ] No — no significant findings from third-party indexing services  
- [ ] Yes — historical snapshots or service scans reveal sensitive metadata or service banners  

---