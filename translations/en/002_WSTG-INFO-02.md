## WSTG-INFO-02 — Fingerprint Web Server

Web server fingerprinting is the process of identifying the specific software type, version, and underlying operating system of a target web server. Attackers perform this reconnaissance to identify known vulnerabilities, such as unpatched CVEs or configuration weaknesses, that are specific to certain versions of Apache, Nginx, IIS, or other server technologies. This is typically achieved by analyzing HTTP response headers (e.g., `Server`, `X-Powered-By`), examining unique protocol behaviors, or observing information leaked through default error pages and files. Successful fingerprinting allows an attacker to tailor their exploitation strategy, moving from generic scanning to high-probability attacks against known architectural flaws.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational / Low* |

> *Severity becomes High if the fingerprinting reveals an outdated or end-of-life (EOL) server version with known critical vulnerabilities.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Are identifying strings present in the HTTP response headers?
- [ ] No — `Server` and `X-Powered-By` headers are **not present** or contain generic values  
- [ ] Yes — headers reveal server software but **not** specific version numbers  
- [ ] Yes — headers reveal specific server software and **exact** version numbers  

### Do default error pages or system-generated responses leak server details?
- [ ] No — custom error pages are used and **do not** disclose server information  
- [ ] Yes — default error pages leak server software name and/or version  
- [ ] Yes — error pages leak server details, version, and **underlying operating system**  

### Is the server version disclosed via default files or directory structures?
- [ ] No — default installation files, manuals, and test scripts have been **removed**  
- [ ] Yes — default files (e.g., `info.php`, `manual/`, `test.html`) are **present** and disclose version data  

### Can the server type be accurately inferred through unique behavior analysis?
- [ ] No — server responses are normalized and behavior-based fingerprinting is **not possible**  
- [ ] Yes — unique behaviors (e.g., header ordering, specific error codes, or TCP/IP stack quirks) **allow** for server identification  

### Is the identified server version currently supported and patched?
- [ ] Yes — the identified version is current and **supported** by the vendor  
- [ ] No — the identified version is **outdated** or **end-of-life (EOL)** with known vulnerabilities  

---