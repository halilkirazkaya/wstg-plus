## WSTG-SESS-07 — Testing Session Timeout

Session timeout testing evaluates whether an application effectively terminates a user's session after a predefined period of inactivity or total duration. This control is vital for mitigating the risk of session hijacking, particularly on shared workstations or in scenarios where an attacker captures a session identifier via network interception or XSS. Pentesters analyze both idle timeouts, which trigger after a period of inactivity, and absolute timeouts, which limit the total lifespan of a session regardless of activity. From an attacker's perspective, indefinite or excessively long sessions provide an extended window of opportunity to maintain unauthorized access and bypass the need for re-authentication.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Does the application implement an idle session timeout?
- [ ] Yes — session expires after a short period (e.g., 15-30 minutes) of inactivity  
- [ ] Yes — session expires but timeout duration is **excessively long** (e.g., > 24 hours)  
- [ ] No — session **remains active** indefinitely during inactivity  

### Is the session timeout enforced on the server-side?
- [ ] Yes — server rejects requests with expired tokens regardless of client-side state  
- [ ] No — timeout is **only** enforced via client-side JavaScript or meta-refresh  

### Does the application implement an absolute session timeout?
- [ ] Yes — session is terminated after a fixed maximum duration regardless of activity  
- [ ] No — session **can** be extended indefinitely as long as there is continuous activity  

### Is the session identifier invalidated on the server upon timeout?
- [ ] Yes — session token **cannot** be reused once the timeout threshold is reached  
- [ ] No — session token **is still valid** on the server even after the client is redirected to the login page  

### Can the session be kept alive indefinitely using automated heartbeat requests?
- [ ] No — absolute timeout or secondary controls **prevent** infinite session extension  
- [ ] Yes — periodic background requests (e.g., AJAX) **can** maintain the session indefinitely  

---