## WSTG-INPV-18 — Testing for Server-Side Template Injection

Server-Side Template Injection (SSTI) occurs when an application embeds user-controlled input into a server-side template engine without sufficient sanitization or escaping. This allows an attacker to inject malicious template directives, which are then executed on the server, potentially leading to full system compromise through remote code execution (RCE). SSTI typically manifests in features like dynamic email generation, profile pages, and notification systems that use engines such as Jinja2, Twig, or FreeMarker. From an attacker's perspective, this vulnerability is often exploited to leak sensitive environment variables, read internal files, or execute system commands by leveraging the template engine's native objects and methods.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Tools:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Does the application use a server-side template engine to process user input?
- [ ] No — application does **not** use server-side templates for dynamic content  
- [ ] Yes — templates are used but user input is **not** embedded in template directives  
- [ ] Yes — user input is embedded in templates **without** proper sanitization  

### Are basic arithmetic expressions evaluated when injected into input fields?
- [ ] No — payloads like `{{7*7}}` or `${7*7}` are reflected as literal strings  
- [ ] Yes — expressions are evaluated (e.g., returning `49`), confirming the template engine **is vulnerable**  

### Can the template engine's built-in objects or methods be accessed?
- [ ] No — access to dangerous objects, methods, or configurations is **restricted** or sandboxed  
- [ ] Yes — built-in objects (e.g., `self`, `config`, `__class__`) **can** be accessed to leak sensitive data  
- [ ] Yes — remote code execution (RCE) via system calls or OS libraries **is possible**  

### Is there a sandbox or allowlist mechanism preventing the execution of dangerous directives?
- [ ] Yes — strict allowlist or secure sandbox is in place and bypass **not possible**  
- [ ] Yes — sandbox is present but bypass **is possible** via specific engine-dependent payloads  
- [ ] No — no sandbox or allowlist **is applied** to the template engine  

---