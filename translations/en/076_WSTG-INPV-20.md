## WSTG-INPV-20 — Mass Assignment

Mass Assignment, also known as Overposting or Insecure Deserialization of parameters, occurs when a web application automatically binds incoming HTTP request parameters to internal object properties without proper attribute filtering. This vulnerability is prevalent in modern MVC frameworks and RESTful APIs where JSON, XML, or form-encoded data is directly deserialized into application-side data models. Attackers exploit this behavior by injecting additional, unlisted parameters into a request—such as `isAdmin`, `role`, or `account_balance`—to modify sensitive fields that the developers did not intend to expose to user control. Successful exploitation typically results in privilege escalation, unauthorized data modification, or the bypassing of critical business logic workflows.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium* |

> *Severity becomes High if administrative attributes, permission levels, or billing fields can be modified.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Does the application use automatic binding for incoming request parameters?
- [ ] No — parameters are manually mapped to specific variables or safe objects  
- [ ] Yes — automatic binding is used but strict **white-lists** or DTOs (Data Transfer Objects) are enforced  
- [ ] Yes — automatic binding is used and sensitive internal attributes **can** be modified  

### Can administrative or permission-related attributes be successfully injected?
- [ ] No — injected parameters are ignored, stripped, or cause a validation error  
- [ ] Yes — extra parameters are accepted but do **not** affect the backend object state  
- [ ] Yes — injecting parameters like `is_admin`, `role`, or `permissions` **successfully** escalates privileges *(Critical)*  

### Is it possible to modify sensitive business logic or financial fields via overposting?
- [ ] No — fields such as `price`, `balance`, or `status` are protected from mass assignment  
- [ ] Yes — sensitive business attributes **can** be modified by adding them to the JSON or form request body  

### Does the application reveal internal object structures that facilitate parameter guessing?
- [ ] No — responses only include intended public fields and documentation is restricted  
- [ ] Yes — internal object fields are visible in GET responses, making parameter discovery **possible**  
- [ ] Yes — API documentation (Swagger/OpenAPI) explicitly lists internal properties that **can** be assigned  

---