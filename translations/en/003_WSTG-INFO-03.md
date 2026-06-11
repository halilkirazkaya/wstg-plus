## WSTG-INFO-03 — Review Webserver Metafiles for Information Leakage

Webserver metafiles such as `robots.txt`, `sitemap.xml`, and `security.txt` are intended to guide crawlers and provide administrative metadata, but they often inadvertently reveal sensitive directory structures or hidden endpoints. Attackers analyze these files to enumerate the application's attack surface, identifying "Disallow" directives that frequently point to unlinked administrative panels, backup directories, or development environments. Beyond search engine instructions, files in the `.well-known/` directory or files like `humans.txt` can disclose technology stacks, developer information, or organizational security contacts. Systematic review of these files allows a tester to gain structural and technical insights without performing aggressive brute-force discovery.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Low / Informational* |

> *Severity becomes Medium if metafiles reveal unauthenticated administrative interfaces or sensitive configuration backups.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Does the `robots.txt` file exist and expose sensitive directories?
- [ ] No — `robots.txt` does **not** exist  
- [ ] Yes — `robots.txt` exists but only contains standard public paths  
- [ ] Yes — `robots.txt` reveals sensitive directory paths or administrative interfaces  
- [ ] Yes — `robots.txt` reveals credentials or specific software versions in comments  

### Is a `sitemap.xml` file present and revealing hidden application structure?
- [ ] No — `sitemap.xml` does **not** exist  
- [ ] Yes — `sitemap.xml` is present but only lists public-facing URLs  
- [ ] Yes — `sitemap.xml` lists non-linked endpoints or internal-only resources that **can** be accessed  

### Are security and organizational metafiles exposing technical details?
- [ ] No — no metadata files found in root or `.well-known/`  
- [ ] Yes — `security.txt` or `humans.txt` are present but contain standard information  
- [ ] Yes — metafiles disclose internal hostnames, developer identities, or technology stack details that **can** aid in social engineering or further attacks  

### Are there legacy or framework-specific metafiles present?
- [ ] No — no framework-specific files (e.g., `info.php`, `composer.json`, `package.json`) found  
- [ ] Yes — files like `README.md`, `CHANGELOG.txt`, or `.DS_Store` are present but sanitized  
- [ ] Yes — framework or version-specific files are present and **allow** for precise technology fingerprinting  

---