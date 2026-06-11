## WSTG-INFO-08 — Fingerprint Web Application Framework

Fingerprinting a web application framework involves identifying the specific technology stack and software versions used to build and serve the application. Attackers analyze HTTP response headers, framework-specific cookies, predictable file structures, and unique JavaScript artifacts to determine if the target is running platforms like React, Angular, Django, or Spring. This reconnaissance phase is vital because it allows a tester to narrow down the attack surface to known vulnerabilities (CVEs) and common configuration weaknesses specific to that framework. By understanding the underlying architecture, an attacker can move from generic testing to highly targeted exploitation strategies.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Are HTTP response headers exposing the framework name or version?
- [ ] No — headers such as `X-Powered-By` or `Server` are **not** present or are sanitized  
- [ ] Yes — framework name is present but version **is hidden**  
- [ ] Yes — framework name and version **are both exposed**  

### Are framework-specific cookies or directory structures identifiable?
- [ ] No — common framework artifacts (e.g., `.js` paths, cookie names) are obfuscated or renamed  
- [ ] Yes — framework-specific cookies (e.g., `JSESSIONID`, `PHPSESSID`, `csrftoken`) **are** identifiable  
- [ ] Yes — directory structures (e.g., `/wp-content/`, `_next/static/`) **are** visible and confirm the framework  

### Do error messages or source code comments disclose framework details?
- [ ] No — error handling is generic and comments are stripped  
- [ ] Yes — framework-specific stack traces **are enabled** and visible in responses  
- [ ] Yes — HTML source code contains comments or metadata tags identifying the framework  

### Are known vulnerabilities associated with the identified framework version?
- [ ] No — the identified framework is up-to-date with **no known** public vulnerabilities  
- [ ] Yes — the framework version is outdated and specific CVEs **can be** identified  

---