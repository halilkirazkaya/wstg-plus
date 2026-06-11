## WSTG-SESS-08 — Session Puzzling

Session Puzzling, also known as Session Variable Overloading, occurs when an application uses the same session variable for multiple purposes across different modules or fails to properly clear session states during workflow transitions. Attackers exploit this behavior by populating session variables in one context—such as an unauthenticated profile preview or a multi-step registration—and then navigating to a protected area where the application incorrectly trusts those pre-existing variables. This can lead to critical impacts including authentication bypass, privilege escalation, or unauthorized access to sensitive data by tricking the application into believing a session is in a higher-privileged state than it actually is. The vulnerability is most prevalent in complex, stateful applications where session management logic is inconsistently applied across different functional components.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Does the application use the same session variable names for different purposes across separate modules?
- [ ] No — variable names are unique or scopes are isolated  
- [ ] Yes — shared variable names exist but state is cleared between modules  
- [ ] Yes — shared variable names exist and state **is not** cleared  

### Can unauthenticated actions influence session variables used in authenticated contexts?
- [ ] No — session variables are initialized only **after** successful authentication  
- [ ] Yes — session variables can be set before login but do **not** affect post-login state  
- [ ] Yes — session variables set during pre-authentication **are** trusted post-authentication *(Critical)*  

### Does the application re-initialize the session identifier and its associated variables upon a change in privilege level?
- [ ] Yes — session is fully reset and a new ID **is issued**  
- [ ] Partial — new ID is issued but some session variables **persist**  
- [ ] No — session ID and all variables **remain** unchanged  

### Can administrative privileges be gained by manipulating session variables via an unprivileged account?
- [ ] No — privilege variables **cannot** be modified by users  
- [ ] Yes — variables can be modified but the application **validates** them against the backend database  
- [ ] Yes — variables can be modified and the application **trusts** the session state implicitly *(Critical)*  

### Is the session state cleared immediately upon completing a specific workflow (e.g., password reset, checkout)?
- [ ] Yes — session variables for the workflow are destroyed upon completion  
- [ ] No — workflow variables **persist** and can be reused in other application areas  

---