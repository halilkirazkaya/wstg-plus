## WSTG-ERRH-01 — Testing for Improper Error Handling

Improper error handling occurs when a web application discloses sensitive technical details—such as stack traces, database schema information, or internal file paths—through its error responses. Attackers trigger these errors by submitting malformed input, accessing non-existent resources, or inducing server-side exceptions to map the application's underlying architecture and identify potential vectors for further exploitation. This information leakage often serves as a precursor to more severe attacks, including SQL injection or path traversal, by providing the attacker with precise environment specifications. A secure implementation must provide generic user-friendly messages while capturing detailed diagnostics only in secure, server-side logs.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Does the application use a generic, global error handler for unhandled exceptions?
- [ ] Yes — generic error pages are **enabled** and reveal **no** technical details  
- [ ] Yes — custom error pages are used but leakage **is possible** via specific headers  
- [ ] No — default server error pages (e.g., Tomcat, IIS, Nginx) are **visible**  

### Can technical information be enumerated through malformed input or boundary cases?
- [ ] No — errors are handled gracefully with unique reference IDs for logging  
- [ ] Yes — stack traces or database queries **are disclosed** in responses  
- [ ] Yes — internal file paths, environment variables, or server version strings **are disclosed**  

### Are API responses leaking verbose error objects or debug information?
- [ ] No — APIs return standardized error codes and sanitized JSON messages  
- [ ] Yes — APIs return verbose debug objects or full exception details in the response body  
- [ ] Yes — stack traces are included in the `details` or `exception` fields of the JSON response  

### Does the application behave differently (Time-based or Content-based) when errors occur?
- [ ] No — response signatures and timings are consistent regardless of the error type  
- [ ] Yes — differential responses **can** be used to enumerate valid vs. invalid states (e.g., User Enumeration)  

---