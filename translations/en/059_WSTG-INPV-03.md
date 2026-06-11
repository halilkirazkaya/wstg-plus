## WSTG-INPV-03 — Testing for HTTP Verb Tampering

HTTP Verb Tampering exploits weaknesses in how web servers and application frameworks authorize access to specific resources based on the HTTP method used. Attackers attempt to bypass security constraints by substituting standard methods like `GET` or `POST` with alternatives such as `HEAD`, `PUT`, `OPTIONS`, or even arbitrary, non-standard strings that the backend might process inconsistently. This vulnerability typically occurs when security configurations—such as Java EE web.xml filters or .NET authorization rules—explicitly list allowed methods but fail to account for others, or when a "deny-by-method" approach is used instead of "deny-all." Successful exploitation can result in unauthorized access to administrative functions, data modification, or information disclosure regarding the server's internal configuration.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if administrative functions or data modification (PUT/DELETE) can be performed without authentication.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Does the application respond to non-standard or arbitrary HTTP methods?
- [ ] No — application rejects all methods except those explicitly required  
- [ ] Yes — application responds to standard methods (OPTIONS, TRACE) but **cannot** bypass security  
- [ ] Yes — application accepts arbitrary strings (e.g., FOO, TEST) and processes them as `GET` or `POST` requests  

### Can authentication/authorization be bypassed by changing the HTTP method?
- [ ] No — security controls are applied globally regardless of the HTTP verb  
- [ ] Yes — controls are in place but bypass **is possible** using the `HEAD` method  
- [ ] Yes — security controls are **not applied** to alternative methods, allowing unauthorized access to restricted endpoints  

### Are dangerous methods such as `PUT` or `DELETE` enabled on the server?
- [ ] No — dangerous methods are **disabled** or return a 405 Method Not Allowed  
- [ ] Yes — methods are enabled but require valid, high-privilege authentication  
- [ ] Yes — methods are **enabled** and allow unauthorized file upload or resource deletion *(Critical)*  

### Does the `OPTIONS` method reveal sensitive information about allowed verbs?
- [ ] No — `OPTIONS` is **disabled** or returns a generic response  
- [ ] Yes — `OPTIONS` returns the `Allow` header but does not expose restricted internal methods  
- [ ] Yes — `OPTIONS` reveals internal-only or administrative methods that aid in further exploitation  

### Is the `TRACE` method enabled, potentially allowing Cross-Site Tracing (XST)?
- [ ] No — `TRACE` and `TRACK` methods are **disabled**  
- [ ] Yes — `TRACE` is **enabled** and reflects back HTTP headers, including sensitive cookies/tokens  

---