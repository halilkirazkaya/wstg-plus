## WSTG-INFO-09 — Fingerprint Web Application

Fingerprinting a web application is the process of identifying the specific framework, Content Management System (CMS), or underlying technology stack and its associated versions. This reconnaissance phase is vital because it allows an attacker to map identified components against known vulnerabilities (CVEs) and public exploits, significantly narrowing the attack surface. Information is typically harvested from HTTP response headers, unique file paths, cookie names, HTML source code metadata, and cryptographic hashes of static assets. By accurately determining the technology stack, an attacker can move from generic automated scanning to highly targeted exploitation of version-specific weaknesses.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Low / Informational |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Are HTTP response headers disclosing the technology stack or version numbers?
- [ ] No — sensitive headers are stripped or contain generic values  
- [ ] Yes — default headers like `X-Powered-By`, `Server`, or `X-AspNet-Version` **are** present  
- [ ] Yes — headers disclose specific, outdated versions of the web server or application framework  

### Is the application framework or CMS identifiable through unique file paths or naming conventions?
- [ ] No — file paths are obfuscated and do **not** reveal the underlying technology  
- [ ] Yes — common directory structures (e.g., `/wp-content/`, `/_next/`, `/node_modules/`) **are** visible  
- [ ] Yes — unique files like `robots.txt`, `humans.txt`, or `sitemap.xml` contain technology-specific metadata  

### Are specific version numbers exposed within the HTML source code or static assets?
- [ ] No — no version information is found in comments or asset parameters  
- [ ] Yes — HTML comments or meta tags (e.g., `<meta name="generator" content="...">`) **are** present  
- [ ] Yes — version strings are appended to CSS/JS filenames (e.g., `jquery.js?v=1.12.4`) and **cannot** be disabled  

### Do default error pages or management interfaces reveal the software environment?
- [ ] No — application uses custom, generic error pages for all status codes  
- [ ] Yes — default error pages for the web server or framework **are** enabled and leak version data  
- [ ] Yes — administrative login portals (e.g., `/wp-admin`, `/phpmyadmin`) **are** accessible and identifiable  

### Are cookies revealing the session management framework?
- [ ] No — session cookies use generic or custom names  
- [ ] Yes — technology-specific cookie names (e.g., `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) **are** used  

---