## WSTG-CONF-05 — Enumerate Infrastructure and Application Admin Interfaces

Administrative interfaces represent the management control plane of an application and its underlying infrastructure, often granting privileged access to system configurations, user data, and server-side operations. Attackers prioritize enumerating these interfaces through directory brute-forcing, analysis of `robots.txt`, or observing predictable URL patterns to find entry points that may lack robust authentication or rely on default credentials. Discovery of an exposed admin panel significantly increases the risk of complete system compromise, unauthorized data modification, or service disruption. These interfaces are frequently found on non-standard ports or subdomains, making them prime targets for automated scanners and manual reconnaissance.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the interface allows system-wide configuration changes, user management, or data exfiltration without multi-factor authentication.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Are administrative interfaces discoverable via directory fuzzing or reconnaissance?
- [ ] No — no administrative interfaces are exposed or discoverable  
- [ ] Yes — interfaces are discoverable but access **is restricted** to internal IP ranges  
- [ ] Yes — interfaces **are** discoverable and publicly accessible  

### Is authentication enforced on discovered administrative portals?
- [ ] Yes — multi-factor authentication (MFA) **is enforced**  
- [ ] Yes — single-factor authentication **is enforced**  
- [ ] No — no authentication is required to access the interface *(Critical)*  

### Are administrative paths hidden or non-standard to prevent discovery?
- [ ] Yes — paths use non-standard, randomized, or non-predictable naming  
- [ ] No — paths use common naming conventions (e.g., `/admin`, `/manager`, `/console`) and **can** be easily guessed  

### Does the interface provide access to sensitive system or application functionality?
- [ ] No — interface is a "read-only" status dashboard with minimal impact  
- [ ] Yes — interface **can** modify application configuration or user data  
- [ ] Yes — interface **can** execute system-level commands or perform database management  

---