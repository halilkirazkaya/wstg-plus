## WSTG-SESS-03 — Session Fixation

Session fixation occurs when an application fails to invalidate or rotate the session identifier after a user successfully authenticates, allowing an attacker to force a known session token onto a victim. If the application preserves the pre-authentication session ID after login, an attacker can provide a victim with a specifically crafted link containing a fixed session ID and subsequently hijack the authenticated session. This vulnerability typically manifests in login forms or via session identifiers accepted through URL parameters and cookies. From an attacker's perspective, exploitation involves "fixing" the session via a malicious link or a header injection vulnerability and waiting for the victim to provide credentials, effectively bypassing the need to steal a live token.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Does the session identifier change after successful authentication?
- [ ] Yes — a new session identifier **is issued** and the old one is invalidated  *(Most secure)*  
- [ ] Yes — a new session identifier **is issued** but the old one **remains valid**  
- [ ] No — the session identifier **remains the same** before and after authentication  *(Critical)*  

### Does the application accept session identifiers provided via URL parameters?
- [ ] No — session IDs are **exclusively** managed via cookies  
- [ ] Yes — session IDs are accepted in the URL but are **ignored** or **rotated** upon login  
- [ ] Yes — session IDs in the URL are **accepted** and **persisted** after authentication  

### Can an attacker-defined session ID be forced onto the application?
- [ ] No — the application **rejects** invalid or non-existent session IDs provided by the user  
- [ ] Yes — the application **accepts** and initializes a session using any arbitrary ID provided in a cookie  
- [ ] Yes — the application **accepts** and initializes a session using any arbitrary ID provided in a URL parameter  

### Are pre-authentication sessions correctly terminated?
- [ ] Yes — the anonymous session is **fully destroyed** upon the login event  
- [ ] No — the anonymous session is **not destroyed**, allowing potential session harvesting  

### Is the session cookie protected against client-side injection?
- [ ] Yes — `HttpOnly` and `Secure` flags are **applied** to prevent script-based fixation  
- [ ] No — flags are **not applied**, allowing session IDs to be set via Cross-Site Scripting (XSS)  

---