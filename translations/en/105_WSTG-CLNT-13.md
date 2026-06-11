## WSTG-CLNT-13 — Testing for Cross Site Script Inclusion

Cross-Site Script Inclusion (XSSI) occurs when a web application exports sensitive data within a dynamically generated JavaScript file or JSONP response, which an attacker-controlled site can then include via a `<script>` tag. Since script inclusion bypasses the Same-Origin Policy (SOP), an attacker can execute the script in their own context and override global objects or variables to exfiltrate the sensitive information. This vulnerability typically manifests in authenticated endpoints that return user-specific data like email addresses, API keys, or session metadata in script format. From an attacker's perspective, exploitation involves crafting a malicious page that references the vulnerable script and captures the resulting global variable changes or function calls.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

> *Severity becomes High if leaked data includes CSRF tokens, session identifiers, or highly sensitive PII.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Do any authenticated endpoints return dynamic JavaScript or JSONP containing sensitive information?
- [ ] No — all scripts are static and **cannot** contain user-specific data  
- [ ] Yes — dynamic scripts exist but **do not** contain sensitive information  
- [ ] Yes — dynamic scripts contain PII or tokens and **are** accessible across origins  

### Is the `X-Content-Type-Options: nosniff` header implemented on sensitive script responses?
- [ ] Yes — header is **applied** and prevents MIME-type sniffing  
- [ ] No — header is **missing**, potentially allowing non-script content to be executed as script  

### Are sensitive scripts protected by anti-XSSI mechanisms such as non-executable prefixes or custom headers?
- [ ] Yes — scripts require a custom header or token that **cannot** be sent via a standard `<script>` tag  
- [ ] Yes — scripts use non-executable prefixes (e.g., `)]}'`) that **cannot** be easily bypassed  
- [ ] No — scripts are directly accessible via GET requests using ambient credentials *(Critical)*  

### Is exfiltration of sensitive data possible by overriding global variables or prototypes?
- [ ] No — sensitive data is scoped locally or protected via modern browser features  
- [ ] Yes — sensitive data is assigned to global variables and **can** be captured  
- [ ] Yes — data is leaked through JSONP callbacks that **can** be intercepted  

---