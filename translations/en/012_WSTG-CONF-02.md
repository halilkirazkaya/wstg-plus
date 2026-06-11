## WSTG-CONF-02 — Test Application Platform Configuration

Application platform configuration testing involves auditing the underlying web server, application server, and framework settings to ensure they are hardened against common exploits. Attackers actively seek out default credentials, unpatched platform vulnerabilities, and exposed administrative interfaces to gain unauthorized access or execute code. Misconfigurations often result in the disclosure of sensitive environment variables, internal system paths, or the presence of unnecessary sample applications that expand the attack surface. From a professional perspective, a poorly configured platform serves as a high-probability entry point for achieving initial access to the target environment.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if administrative interfaces are accessible with default credentials or if sample scripts allow for Remote Code Execution.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Are default credentials or accounts active on the platform?
- [ ] No — all default accounts are **disabled** or passwords changed  
- [ ] Yes — default accounts exist but are **not accessible** from the external network  
- [ ] Yes — default accounts are **active** and accessible via the internet *(Critical)*  

### Is the platform disclosing sensitive version information via headers or error pages?
- [ ] No — version strings are **hidden** or generic  
- [ ] Yes — detailed version information **is disclosed** in HTTP headers (e.g., `Server`, `X-Powered-By`)  
- [ ] Yes — full stack traces and internal system paths **are exposed** in verbose error messages  

### Are administrative or management interfaces properly restricted?
- [ ] No — administrative interfaces are **not exposed**  
- [ ] Yes — interfaces exist but are **restricted** by IP allowlisting or Multi-Factor Authentication  
- [ ] Yes — interfaces (e.g., `/admin`, `/manager`, `/console`) are **publicly accessible** with single-factor auth  

### Are sample files, scripts, or documentation present on the production server?
- [ ] No — all non-essential files have been **removed**  
- [ ] Yes — sample files or documentation **are present** but do not leak sensitive info  
- [ ] Yes — sample scripts (e.g., `info.php`, `examples/`) **are present** and provide a direct attack vector  

### Is the server configured to support insecure HTTP methods?
- [ ] No — only `GET` and `POST` are **enabled**  
- [ ] Yes — potentially risky methods like `PUT`, `DELETE`, or `TRACE` are **enabled** but restricted  
- [ ] Yes — risky methods are **enabled** and allow for unauthorized file modification or credential theft  

---