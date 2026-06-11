## WSTG-INPV-06 — Testing for LDAP Injection

LDAP Injection occurs when an application incorporates user-supplied data into LDAP (Lightweight Directory Access Protocol) filters without proper sanitization or escaping, allowing an attacker to manipulate the query logic. By injecting special characters such as asterisks, parentheses, and logical operators, attackers can modify the search filter to bypass authentication mechanisms or exfiltrate sensitive directory information including usernames, group memberships, and organizational attributes. This vulnerability typically manifests in features like corporate directory searches, employee portals, or single sign-on (SSO) systems that interface with Active Directory or OpenLDAP. From an attacker's perspective, successful exploitation often involves blind techniques to enumerate the directory structure or attribute values bit-by-bit when direct query output is suppressed by the application.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Does the application utilize LDAP for authentication or directory searches?
- [ ] No — LDAP is **not used** in the application architecture  
- [ ] Yes — LDAP is used and all inputs are strictly sanitized or parameterized  
- [ ] Yes — LDAP is used and user input is concatenated into filters **without** proper escaping  

### Are LDAP meta-characters properly escaped or filtered in user input?
- [ ] Yes — characters such as `(`, `)`, `&`, `|`, `*`, and `\` are **not allowed** or are correctly escaped  
- [ ] No — meta-characters **can** be injected to alter the structure of the LDAP filter  

### Can the authentication mechanism be bypassed via injection?
- [ ] No — login logic is **not susceptible** to query manipulation  
- [ ] Yes — authentication **can** be bypassed using logical OR (`|`) or wildcard (`*`) injection in the username or password fields  

### Is blind data exfiltration possible from the directory service?
- [ ] No — search results are limited and no timing or boolean differences are observed  
- [ ] Yes — directory attributes **can** be enumerated bit-by-bit via boolean-based blind injection techniques  
- [ ] Yes — full directory records **are** reflected in the application response due to query manipulation  

### Are secondary application components (e.g., mailers, SSO) vulnerable to LDAP-based attribute injection?
- [ ] No — LDAP attributes are handled securely across all components  
- [ ] Yes — an attacker **can** modify their own attributes (e.g., email, group membership) via injection to escalate privileges or redirect communications  

---