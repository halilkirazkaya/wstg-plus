## WSTG-ATHZ-03 — Testing for Privilege Escalation

Privilege escalation occurs when an attacker exploits vulnerabilities to gain access to resources or functionality reserved for users with higher authorization levels or different identities. In vertical escalation, a standard user attempts to access administrative functions, while horizontal escalation involves accessing data belonging to another user with the same privilege level. These flaws typically manifest in poorly implemented access control lists (ACLs), insecure direct object references (IDOR), or parameter tampering within session or identity tokens. From an attacker's perspective, this is often achieved by manipulating numeric IDs, modifying role-based parameters in cookies or JWTs, or directly calling hidden API endpoints that lack server-side authorization checks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Does the application implement multiple privilege levels or multi-tenancy?
- [ ] No — application is a single-user or single-role system  
- [ ] Yes — multiple roles or tenants **are** defined and require authorization testing  

### Is horizontal privilege escalation (same-level access) possible?
- [ ] No — authorization checks **are applied** to all resource identifiers and session owners  
- [ ] Yes — access to other users' data **is possible** through IDOR or parameter tampering  
- [ ] Yes — data leakage **is possible** but modification of other users' data **is not possible**  

### Is vertical privilege escalation (low-to-high) possible?
- [ ] No — administrative endpoints strictly enforce role-based access controls (RBAC)  
- [ ] Yes — administrative functions **can** be accessed by low-privileged users via direct URL access  
- [ ] Yes — administrative functions **can** be accessed by manipulating role-related headers, cookies, or JWT claims  

### Can user roles or permissions be modified via Mass Assignment or Parameter Pollution?
- [ ] No — role-based fields are strictly read-only and **cannot** be modified by users  
- [ ] Yes — user permissions **can** be elevated by including hidden parameters (e.g., `role=admin`) in profile updates or registration  

### Are authorization checks consistently enforced across all API versions and HTTP methods?
- [ ] Yes — controls **are applied** consistently across all versions and methods  
- [ ] No — legacy API versions (e.g., `/v1/`) or specific HTTP methods (e.g., `PUT`, `DELETE`, `PATCH`) **bypass** authorization  
- [ ] No — administrative functions are hidden in the UI but **enabled** at the API level for all users  

---