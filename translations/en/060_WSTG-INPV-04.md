## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) occurs when an application receives multiple HTTP parameters with the same name and processes them in an inconsistent or insecure manner. By supplying duplicate parameters, an attacker can manipulate the application's internal logic, potentially bypassing Web Application Firewalls (WAF), input validation filters, or access control mechanisms. This vulnerability is highly dependent on the underlying web server or application framework, which may choose the first, last, or a concatenated version of the repeated parameters. Exploitation typically targets endpoints that pass parameters to back-end APIs, database queries, or URL redirection mechanisms, leading to impacts ranging from cross-site scripting (XSS) to unauthorized data exfiltration.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### How does the application handle multiple HTTP parameters with the same name?
- [ ] No — duplicate parameters are **rejected** or ignored by the server  
- [ ] Yes — the server consistently selects only the **first** or **last** occurrence  
- [ ] Yes — the server **concatenates** values, potentially allowing for injection  

### Can HPP be leveraged to bypass security controls like a WAF or input filter?
- [ ] No — security controls correctly inspect **all** occurrences of a parameter  
- [ ] Yes — a WAF or filter only inspects the **first** occurrence, allowing a malicious **second** occurrence to pass  
- [ ] Yes — concatenation allows an attacker to **split** a payload across multiple parameters to evade detection  

### Does the application reflect polluted parameters in the response without proper sanitization?
- [ ] No — parameters are sanitized or encoded before being reflected in the UI  
- [ ] Yes — pollution results in reflected content, but sanitization **is applied**  
- [ ] Yes — pollution results in reflected content and sanitization **is not applied**, leading to XSS or logic errors  

### Are parameters passed to internal API calls or back-end systems vulnerable to pollution?
- [ ] No — back-end requests use safe, non-dynamic construction methods  
- [ ] Yes — HPP allows an attacker to **inject** or **overwrite** parameters in the back-end request structure  

### Does the application exhibit different behavior when parameters are polluted in GET vs POST requests?
- [ ] No — behavior is consistent across all HTTP methods  
- [ ] Yes — only GET parameters are vulnerable to pollution  
- [ ] Yes — only POST (body) parameters are vulnerable to pollution  

---