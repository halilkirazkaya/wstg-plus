## WSTG-ERRH-02 — Testing for Stack Traces

Stack traces are generated when an application fails to gracefully handle an exception, resulting in the disclosure of the internal execution state and call hierarchy to the end user. For a penetration tester, these traces are invaluable as they reveal the application framework, library versions, internal file system paths, and logic flow, which significantly reduces the effort required for reconnaissance. Exploitation involves intentionally triggering errors via malformed input, unexpected data types, or boundary condition violations to force the application into an unstable state and exfiltrate information about the underlying technology stack. This leakage often provides the necessary context to identify vulnerable third-party components or to craft specific exploits for logic flaws discovered within the disclosed code paths.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity increases to Medium if the stack trace reveals sensitive architectural details, database queries, or internal configuration parameters.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Are full stack traces returned to the client upon application errors?
- [ ] No — application returns generic error pages or custom error messages  
- [ ] Yes — stack traces are **enabled** but only for specific, non-sensitive modules  
- [ ] Yes — full stack traces are **enabled** and visible in the HTTP response body  

### Do error messages disclose sensitive environmental information?
- [ ] No — error messages contain no technical or environmental details  
- [ ] Yes — messages reveal **internal file paths** or server-side **directory structures**  
- [ ] Yes — messages reveal **database schemas**, **SQL queries**, or **third-party library versions**  

### Can stack traces be triggered by manipulating input parameters?
- [ ] No — malformed input is handled gracefully or rejected by validation  
- [ ] Yes — traces can be triggered by **unexpected data types** (e.g., submitting an array where a string is expected)  
- [ ] Yes — traces are triggered by **null bytes**, **special characters**, or **boundary values**  

### Is a global error handler consistently applied across the application?
- [ ] Yes — a global exception handler is **consistently applied** across all tested endpoints  
- [ ] Yes — a handler is present but **not applied** to legacy, API, or newly added endpoints  
- [ ] No — no global error handling mechanism is **detected**, resulting in default server errors  

---