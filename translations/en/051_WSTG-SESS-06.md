## WSTG-SESS-06 — Testing for Logout Functionality

Logout functionality is a critical security control designed to terminate a user's session and invalidate associated session identifiers on both the client and server sides. Failure to properly implement logout allows session fixation or hijacking attacks, as an attacker who gains access to a machine after a user has "logged out" could potentially reuse the still-active session token. Pentesters evaluate this by checking if the session cookie is cleared from the browser, if the server-side session state is explicitly destroyed, and if back-button navigation allows access to cached sensitive data. A secure logout implementation ensures that once the action is triggered, no subsequent requests with the old session token are authorized by the application.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Is a functional logout mechanism present and accessible?
- [ ] Yes — logout button exists and triggers a termination request  
- [ ] No — logout button is **missing** or does **not** trigger a termination event  

### Is the session identifier invalidated on the server side?
- [ ] Yes — server rejects all subsequent requests using the old session token  
- [ ] No — server **continues to accept** the old session token after logout  

### Does the application clear session cookies in the browser upon logout?
- [ ] Yes — cookies are overwritten with an expired date or empty value  
- [ ] Yes — cookies remain but are **no longer valid** on the server  
- [ ] No — cookies remain in the browser and **retain** their original values  

### Can sensitive authenticated content be accessed via the browser 'Back' button after logout?
- [ ] No — `Cache-Control: no-store` or similar headers prevent viewing sensitive pages post-logout  
- [ ] Yes — sensitive pages are cached and **can** be viewed by navigation after the session is terminated  

---