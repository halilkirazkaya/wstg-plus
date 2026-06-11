## WSTG-ATHN-07 — Testing for Weak Password Policy

Testing for weak password policies involves evaluating the complexity, length, and entropy requirements enforced by an application during account creation and credential updates. Insufficient policies directly compromise the confidentiality of user accounts by making them susceptible to automated dictionary attacks, brute-force attempts, and credential stuffing. These vulnerabilities typically reside in registration endpoints, password reset workflows, and administrative user management panels. From an attacker's perspective, a permissive policy significantly reduces the computational cost and time required to successfully compromise credentials using common wordlists or pattern-based cracking.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Test Status** | Not Performed |
| **Severity** | Medium / Low* |

> *Severity becomes High if the application lacks account lockout or rate-limiting mechanisms to prevent automated guessing.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Is a minimum password length enforced by the application?
- [ ] Yes — minimum length is 12+ characters *(Most secure)*  
- [ ] Yes — minimum length is between 8 and 11 characters  
- [ ] Yes — minimum length is **less than** 8 characters  
- [ ] No — no minimum length is enforced  

### Are complexity requirements (uppercase, lowercase, digits, symbols) enforced?
- [ ] Yes — multiple character classes are mandatory and bypass **not possible**  
- [ ] Yes — character classes are suggested but **not** enforced  
- [ ] No — no complexity requirements are applied  

### Does the application block common/weak passwords or passwords containing user-specific data?
- [ ] Yes — known weak passwords (e.g., "password123") and username-based passwords **cannot** be used  
- [ ] Yes — common passwords are blocked, but user-specific data (e.g., username, email) **can** be used  
- [ ] No — any string meeting length/complexity requirements is accepted  

### Can complexity or length requirements be bypassed via client-side modification?
- [ ] No — policies are strictly enforced on the **server-side**  
- [ ] Yes — policies are enforced via JavaScript and **can** be bypassed by intercepting the request  

### Is the password policy clearly communicated to the user without leaking implementation details?
- [ ] Yes — policy is visible during input and provides generic failure messages  
- [ ] Yes — policy is visible but failure messages reveal specific regex or logic gaps  
- [ ] No — policy is **not** visible, forcing trial-and-error discovery  

---