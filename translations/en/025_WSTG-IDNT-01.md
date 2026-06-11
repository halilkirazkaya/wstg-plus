## WSTG-IDNT-01 — Test Role Definitions

Testing for role definitions involves identifying and documenting the various user roles and permission levels defined within the application to ensure they are logically separated and adhere to the principle of least privilege. An attacker analyzes the application to map out high-privilege roles versus standard user roles, looking for any ambiguity in permission boundaries or undocumented administrative functions. Identifying these roles is the first step in discovering privilege escalation paths, as inconsistencies in role definitions often lead to unauthorized access to sensitive data or administrative features. This phase typically occurs during initial reconnaissance and business logic mapping, focusing on user profiles, administrative dashboards, and API permission structures.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Are roles clearly defined and documented in the application or its documentation?
- [ ] Yes — roles are fully documented and follow the principle of **least privilege**  
- [ ] Yes — roles are documented but contain **overlapping permissions**  
- [ ] No — no formal role documentation or definitions exist  

### Is there a clear separation between administrative and standard user functions?
- [ ] Yes — administrative functions are **completely isolated** from standard user views  
- [ ] Yes — separation is present but administrative endpoints are **predictable or guessable**  
- [ ] No — administrative and standard functions are **mixed** within the same interfaces  

### Does the application use a centralized mechanism for Role-Based Access Control (RBAC)?
- [ ] Yes — centralized RBAC or ABAC is **implemented** and consistently enforced  
- [ ] Yes — decentralized controls exist but are **inconsistently applied** across modules  
- [ ] No — access control logic is **hardcoded** into individual pages or components  

### Are there undocumented or hidden roles present in the system?
- [ ] No — all discovered roles match documented functionality  
- [ ] Yes — hidden or "shadow" roles (e.g., `super_admin`, `support`, `test`) **are present**  

### Can a low-privileged user enumerate the permissions or roles of other users?
- [ ] No — users **cannot** view or enumerate the roles/permissions of other entities  
- [ ] Yes — users **can** enumerate roles through profile pages, metadata, or API responses  *(Medium)*  

---