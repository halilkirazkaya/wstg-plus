## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), also known as Insecure Direct Object Reference (IDOR), occurs when an API fails to validate whether a user has the appropriate permissions to access or manipulate a specific resource identified by an ID. Attackers exploit this by systematically enumerating or guessing identifiers—such as numeric IDs or UUIDs—in request paths, query parameters, or JSON bodies to access data belonging to other users. This vulnerability is the most common and impactful issue in modern API security, potentially leading to mass data exfiltration, unauthorized modification, or complete account takeover. From an attacker's perspective, the goal is to identify endpoints that accept object identifiers and test if the server-side authorization logic is missing or can be bypassed by manipulating those IDs.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Are object identifiers predictable or enumerable within API requests?
- [ ] No — identifiers are long, random, and cryptographically secure (e.g., UUIDv4)  
- [ ] Yes — identifiers are sequential integers (e.g., `101`, `102`)  
- [ ] Yes — identifiers follow a predictable pattern or are derived from public information  

### Does the API perform server-side validation of object ownership for every request?
- [ ] Yes — authorization checks are applied for every request and bypass **not possible**  
- [ ] Yes — checks are applied but bypass **is possible** via parameter pollution or method tunneling  
- [ ] No — application relies solely on the user being authenticated without checking specific resource ownership  

### Is it possible to access or modify another user's resource by changing the identifier?
- [ ] No — unauthorized access to other users' resources is **not possible**  
- [ ] Yes — unauthorized **read** access (Horizontal BOLA) **is possible**  
- [ ] Yes — unauthorized **modification** or **deletion** (Horizontal BOLA) **is possible**  

### Does the API allow accessing administrative or system-level objects by substituting IDs?
- [ ] No — administrative resources are protected by secondary authorization layers  
- [ ] Yes — accessing or modifying system-level resources (Vertical BOLA) **is possible**  

### Can the authorization check be bypassed by moving the identifier to a different part of the request?
- [ ] No — authorization logic is consistent regardless of parameter location  
- [ ] Yes — bypass **is possible** when the ID is moved from the URL path to the JSON body or headers  
- [ ] Yes — bypass **is possible** when multiple IDs are provided and the server processes the unauthorized one  

---