## WSTG-ATHN-10 — Testing for Weaker Authentication in Alternative Channel

Testing for weaker authentication in alternative channels involves identifying and analyzing secondary paths to account access—such as mobile APIs, password recovery workflows, help desk interfaces, or legacy subdomains—that may not enforce the same security rigor as the primary web portal. These alternative channels often lack robust multi-factor authentication (MFA), strict rate limiting, or complex password requirements, creating a "weakest link" that undermines the entire authentication system. Attackers specifically target these overlooked entry points to perform credential stuffing or bypass MFA by exploiting discrepancies in how different interfaces handle identity verification. Ensuring parity across all authentication surfaces is essential to prevent unauthorized access and maintain the integrity of the user's session and data.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Are alternative authentication channels present and identified?
- [ ] No — only a single authentication channel exists  
- [ ] Yes — multiple channels identified (e.g., mobile app API, desktop client, legacy portal, SOAP services)  

### Do alternative channels enforce security parity with the primary channel?
- [ ] Yes — all channels enforce **identical** password complexity and MFA requirements  
- [ ] Yes — channels enforce password complexity but MFA is **not required** or **can** be bypassed  
- [ ] No — alternative channels have **significantly weaker** authentication requirements  

### Is rate limiting and account lockout consistent across all channels?
- [ ] Yes — rate limiting and lockout are **consistently enforced** across all endpoints  
- [ ] Yes — controls are present but bypass **is possible** on specific channels (e.g., mobile API)  
- [ ] No — rate limiting or lockout is **not applied** to secondary authentication paths  

### Can MFA be bypassed by switching to an alternative channel?
- [ ] No — MFA is mandatory and **cannot** be bypassed regardless of the entry point  
- [ ] Yes — MFA is required on the web portal but **not enforced** on the mobile or legacy API  

### Are password recovery or "forgot password" flows weaker than the standard login?
- [ ] No — recovery flows use strong, out-of-band verification and do not leak information  
- [ ] Yes — recovery flows use weak security questions that **can** be easily guessed or researched  
- [ ] Yes — recovery flows are vulnerable to enumeration or **do not** require the same level of proof as a standard login  

---