## WSTG-INPV-09 — Testing for XPath Injection

XPath Injection occurs when an application uses user-supplied information to construct an XPath query for XML data, allowing an attacker to interfere with the query's logic. By injecting specific syntax such as single quotes, brackets, and logical operators, an attacker can navigate the XML document structure, bypass authentication, or exfiltrate sensitive data nodes. This vulnerability is commonly found in web services, configuration management interfaces, and legacy systems that rely on XML-based data stores rather than traditional SQL databases. From an attacker's perspective, this is often exploited using boolean-based or error-based techniques to systematically map out the XML tree and retrieve hidden values from the backend.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Tools:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Are user inputs used to construct XPath queries properly sanitized or parameterized?
- [ ] Yes — inputs are **not** used in XPath queries or are strictly parameterized  
- [ ] Yes — robust input validation/encoding **is applied** and bypass **not possible**  
- [ ] Yes — some validation **is applied** but bypass **is possible** using encoded characters  
- [ ] No — user input is directly concatenated into XPath queries *(Critical)*  

### Does the application reveal XML/XPath structural details via error messages?
- [ ] No — generic error pages are shown and **no** XML details are leaked  
- [ ] Yes — error messages reveal the presence of XML processing but **no** path details  
- [ ] Yes — verbose error messages reveal XPath syntax, node names, or file paths  

### Is it possible to bypass authentication or authorization logic using XPath logic?
- [ ] No — authentication logic does **not** rely on XML/XPath or is securely implemented  
- [ ] Yes — login or permission checks can be bypassed using logic-based payloads like `' or 1=1 or '`  

### Can the XML document structure or data be enumerated via blind injection?
- [ ] No — application is **not** susceptible to boolean-based or time-based inference  
- [ ] Yes — node names and data **can** be exfiltrated using boolean-based inference  
- [ ] Yes — full XML tree traversal and data extraction **is possible**  

---