## WSTG-IDNT-03 — Test Account Provisioning Process

The account provisioning process governs how new identities are created, verified, and assigned privileges within an application environment. Attackers target this workflow to perform automated mass-registrations for spam or denial-of-service, bypass identity verification steps to create fraudulent accounts, or manipulate parameters to grant themselves elevated permissions during the signup phase. Vulnerabilities often manifest in self-service registration forms, invitation-only systems, or administrative provisioning consoles where business logic flaws allow for unauthorized account creation or privilege escalation. From an attacker's perspective, compromising the provisioning flow provides a legitimate foothold that can be used as a staging ground for deeper exploitation, data exfiltration, or social engineering attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes Critical if an attacker can provision administrative accounts or bypass global registration restrictions.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Is self-registration strictly controlled or limited to authorized users?
- [ ] Yes — registration is **disabled** or requires manual administrative approval  
- [ ] Yes — registration is open but restricted to **authorized** email domains or invitation tokens  
- [ ] No — registration is **open** to any user with no domain or token restrictions  

### Is a robust identity verification mechanism (e.g., email/SMS) required before account activation?
- [ ] Yes — verification is **required** and **cannot** be bypassed  
- [ ] Yes — verification is **required** but **can** be bypassed via parameter tampering or predictable tokens  
- [ ] No — accounts are **active immediately** upon submission without any verification  

### Can the user manipulate role or permission parameters during the registration request?
- [ ] No — roles are **assigned server-side** and no client-side parameters exist  
- [ ] Yes — role parameters exist but manipulation is **not possible** due to server-side validation  
- [ ] Yes — role/permission parameters (e.g., `role=admin`, `is_staff=true`) **can** be modified to gain elevated access *(Critical)*  

### Are rate limits or anti-automation controls implemented to prevent mass account creation?
- [ ] Yes — rate limits and CAPTCHAs are **active** and effective  
- [ ] Yes — controls exist but bypass **is possible** via header spoofing or CAPTCHA solving services  
- [ ] No — no rate limits or anti-automation controls are **applied**  

### Does the provisioning process leak information about existing accounts (User Enumeration)?
- [ ] No — response messages are **identical** for existing and non-existing users  
- [ ] Yes — different responses or timing variations **allow** for the enumeration of existing users  

---