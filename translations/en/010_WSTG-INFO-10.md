## WSTG-INFO-10 — Map Application Architecture

Mapping the application architecture involves identifying the underlying technologies, infrastructure components, and data flow paths that support the web application. By analyzing HTTP response headers, error messages, cookie formats, and file extensions, testers can reconstruct a schematic of the web servers, application servers, databases, and third-party integrations in use. This knowledge is fundamental for tailoring subsequent attacks, as it allows for the selection of platform-specific exploits and the identification of potential bottlenecks or misconfigured intermediary devices like load balancers and WAFs. An attacker leverages this structural map to move from generic scanning to targeted exploitation of the specific software stack and its interconnected components.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational / Low* |

> *Severity becomes Medium if internal network topology or private IP addresses are disclosed.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Can the web server and application server technologies be accurately identified?
- [ ] No — no identification **possible** due to effective hardening or obfuscation  
- [ ] Yes — technology type is known but specific versions **cannot** be determined  
- [ ] Yes — technology and specific versions **are** fully disclosed via headers or file signatures *(Informational)*  

### Are intermediary devices (WAFs, Load Balancers, Proxies) detectable in the communication path?
- [ ] No — no intermediaries are detected or they are fully transparent  
- [ ] Yes — existence is suspected via timing or behavior, but identity **is not** confirmed  
- [ ] Yes — specific products (e.g., Cloudflare, Nginx, F5) **are** identified via unique headers or behavior  

### Does the application leak information about its internal directory structure or network topology?
- [ ] No — internal paths and IPs are **not** disclosed  
- [ ] Yes — internal paths are leaked in error messages or stack traces but internal IPs **are not**  
- [ ] Yes — both internal directory structures and private IP addresses **are** disclosed  

### Can the backend database type and version be inferred from application behavior?
- [ ] No — database details **cannot** be determined  
- [ ] Yes — database type (e.g., PostgreSQL, MSSQL) is inferred via error messages or behavior  
- [ ] Yes — database type and version **are** explicitly disclosed in response bodies or headers  

### Is the application hosted on identifiable cloud service providers or specific third-party CDNs?
- [ ] No — hosting infrastructure is **not** identifiable  
- [ ] Yes — cloud provider (AWS, Azure, GCP) is identified but specific services **are not**  
- [ ] Yes — specific cloud services (e.g., S3 buckets, Lambda, Azure Functions) **are** mapped and reachable  

---