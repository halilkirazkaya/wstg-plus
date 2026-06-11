## WSTG-IDNT-02 — Test User Registration Process

Testing the user registration process involves evaluating the workflow through which new identities are created to ensure it cannot be abused for unauthorized access or automated exploitation. Vulnerabilities in this process often manifest as lack of identity verification (email/SMS), susceptibility to mass account creation via bots, or information disclosure through verbose error messages that reveal existing usernames. Attackers exploit these flaws to perform user enumeration, conduct large-scale spam campaigns, or manipulate registration parameters to gain elevated privileges during the account creation phase. This test is vital for protecting the application's integrity and preventing attackers from populating the system with fraudulent or malicious accounts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Is identity verification required before a new account becomes active?
- [ ] Yes — registration requires a unique, time-bound token sent via email or SMS  
- [ ] Yes — verification is required but tokens are **weak** or **predictable**  
- [ ] No — accounts are activated **immediately** without any verification step  

### Does the registration process prevent user enumeration?
- [ ] Yes — the application returns **identical** responses for both new and existing emails/usernames  
- [ ] No — error messages (e.g., "Email already in use") **allow** an attacker to map registered users  
- [ ] No — timing differences in server responses **reveal** whether a user exists  

### Are anti-automation controls implemented to prevent mass registration?
- [ ] Yes — CAPTCHA or proof-of-work mechanisms are **present** and **effective**  
- [ ] Yes — controls exist but bypass **is possible** via API endpoints or header manipulation  
- [ ] No — no rate limiting or CAPTCHA is **applied** to the registration endpoint  

### Can registration parameters be manipulated to escalate privileges?
- [ ] No — user roles are assigned **strictly** by the server-side logic  
- [ ] Yes — parameters such as `role`, `admin`, or `group_id` **can** be modified in the request to gain higher access  

### Is the password policy enforced during the registration phase?
- [ ] Yes — strong password requirements are **enforced** server-side  
- [ ] No — weak passwords (e.g., "password123") are **accepted** by the system  

---