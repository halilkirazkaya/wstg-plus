## WSTG-ATHZ-02 — Testing for Bypassing Authorization Schema

Bypassing authorization schemas occurs when an application fails to enforce access controls, allowing an attacker to access resources or functions outside their intended permissions. This vulnerability typically manifests as Horizontal Privilege Escalation, where an attacker accesses data belonging to another user of the same rank, or Vertical Privilege Escalation, where a low-privileged user gains administrative capabilities. Attackers exploit these flaws by manipulating request parameters such as resource IDs, modifying session tokens, or leveraging HTTP headers like `X-Original-URL` to circumvent path-based restrictions. Ensuring robust authorization is critical for maintaining data confidentiality and preventing unauthorized administrative actions that could compromise the entire system.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Is horizontal privilege escalation prevented between users of the same role?
- [ ] Yes — authorization checks **are applied** to every resource request and bypass **not possible**  
- [ ] Yes — authorization checks **are applied** but can be bypassed via IDOR or parameter tampering  
- [ ] No — users **can** access data belonging to other users by simply changing a resource ID *(High)*  

### Is vertical privilege escalation prevented for low-privileged users?
- [ ] No — application has only one privilege level  
- [ ] Yes — administrative functions **cannot** be accessed by low-privileged users  
- [ ] Yes — administrative functions are hidden in the UI but **can** be accessed via direct URL  
- [ ] No — low-privileged users **can** perform administrative actions by manipulating roles or permissions *(Critical)*  

### Can authorization be bypassed using HTTP verb tampering?
- [ ] No — access controls are enforced regardless of the HTTP method used  
- [ ] Yes — changing the method (e.g., from `GET` to `POST` or `PUT`) **bypasses** authorization checks  

### Are administrative or restricted endpoints accessible without any authentication?
- [ ] No — all sensitive endpoints require a valid session and proper permissions  
- [ ] Yes — some administrative endpoints are accessible if the attacker knows the direct URL  
- [ ] Yes — unauthenticated access to sensitive functions **is possible** *(Critical)*  

### Do HTTP headers allow for the circumvention of path-based authorization?
- [ ] No — the application is **not** susceptible to header-based bypasses  
- [ ] Yes — headers such as `X-Original-URL`, `X-Rewrite-URL`, or `X-Forwarded-For` **can** be used to bypass access controls  

---