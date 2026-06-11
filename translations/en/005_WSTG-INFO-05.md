## WSTG-INFO-05 — Review Webpage Content for Information Leakage

Reviewing webpage content involves the manual and automated inspection of application responses, including HTML, JavaScript, and CSS files, to identify sensitive information unintentionally exposed to users. Attackers scrutinize client-side source code for developer comments, hidden form fields, internal server paths, hardcoded API keys, and debugging information that can aid in further exploitation phases. This leakage often occurs when developers forget to strip metadata or debugging code before moving to production, potentially revealing logic flaws or backend infrastructure details. From an attacker's perspective, this provides a low-effort method to map the application's internal structure and discover overlooked entry points without triggering security alerts or interacting with the server's backend logic.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity becomes High if sensitive hardcoded credentials, private keys, or internal environment variables are discovered.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Are developer comments containing sensitive information present in the source code?
- [ ] No — no comments or only generic, non-sensitive comments found  
- [ ] Yes — comments exist but contain **no** sensitive logic or data  
- [ ] Yes — comments **reveal** internal paths, SQL queries, or administrative instructions  
- [ ] Yes — comments **reveal** credentials or sensitive developer notes *(High)*  

### Do hidden HTML input fields contain sensitive data or state information?
- [ ] No — no hidden fields found or they contain only non-sensitive tokens  
- [ ] Yes — hidden fields are used for state but are **encrypted** or **signed**  
- [ ] Yes — hidden fields contain **plaintext** passwords, user roles, or internal IDs  

### Are hardcoded secrets, API keys, or internal IP addresses present in JavaScript files?
- [ ] No — no sensitive strings or secrets found in JS files  
- [ ] Yes — JS files contain public API keys with **proper** restrictions  
- [ ] Yes — JS files contain **unprotected** API keys, internal IPs, or private endpoints  
- [ ] Yes — JS files contain **hardcoded** administrative credentials or private keys *(Critical)*  

### Does the application reveal framework versions or internal file paths in the source?
- [ ] No — framework information and file paths are **minified** or **obfuscated**  
- [ ] Yes — framework names and versions are **visible** but patched  
- [ ] Yes — internal file paths or absolute server paths are **exposed** in source code  

### Is the source code leak-prone due to the lack of minification or obfuscation?
- [ ] No — production code is **fully minified** and stripped of comments  
- [ ] Yes — code is minified but comments **remain** in the source  
- [ ] Yes — source maps (`.map` files) are **publicly accessible**, allowing full reconstruction of the source code  

---