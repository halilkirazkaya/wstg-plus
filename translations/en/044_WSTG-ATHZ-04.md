## WSTG-ATHZ-04 — Testing for Insecure Direct Object References

Insecure Direct Object References (IDOR) occur when an application provides direct access to objects based on user-supplied input without performing an authorization check to verify the requester's permissions. This vulnerability significantly impacts confidentiality and integrity, as it allows attackers to view, modify, or delete data belonging to other users or the system. It typically manifests in URL parameters, POST body data, or JSON keys that refer to internal database keys, filenames, or account identifiers. From an attacker's perspective, exploitation involves manipulating these identifiers—often through incrementing integers or fuzzing UUIDs—to access resources outside their authorized scope.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Does the application use direct object identifiers in request parameters?
- [ ] No — application uses indirect references or encrypted maps *(Most secure)*  
- [ ] Yes — application uses non-predictable identifiers (e.g., long UUIDs/hashes)  
- [ ] Yes — application uses predictable identifiers (e.g., incremental integers or usernames)  

### Is server-side authorization performed for every request involving an object identifier?
- [ ] Yes — server-side checks **cannot** be bypassed for any identifier  
- [ ] Yes — server-side checks are present but bypass **is possible** via parameter pollution or header manipulation  
- [ ] No — no authorization check **is applied** to verify ownership of the requested object  

### Can an attacker access or modify objects belonging to other users (Horizontal IDOR)?
- [ ] No — access to other users' data **is not possible**  
- [ ] Yes — viewing of other users' data **is possible** but modification **is not possible**  
- [ ] Yes — both viewing and modification of other users' data **is possible** *(Critical)*  

### Is it possible to access administrative or system-level objects (Vertical IDOR)?
- [ ] No — access to higher-privileged objects **is not possible**  
- [ ] Yes — access to administrative configurations or system files **is possible**  

### Does the application leak object identifiers via search, metadata, or other endpoints?
- [ ] No — identifiers are **not exposed** unintentionally  
- [ ] Yes — identifiers for other users/objects **can** be enumerated through secondary endpoints or public profiles  

---