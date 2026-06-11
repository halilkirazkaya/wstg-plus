## WSTG-SESS-11 — Testing for Concurrent Sessions

Concurrent session testing evaluates whether an application permits a single user account to maintain multiple simultaneous active sessions across different browsers, devices, or IP addresses. Failure to restrict concurrent sessions increases the window of opportunity for attackers to utilize hijacked session tokens or compromised credentials without alerting the legitimate user or terminating existing sessions. This behavior is typically assessed by authenticating from multiple sources and monitoring session stability, often revealing weaknesses in session management logic or a lack of security-focused business rules. From an attacker's perspective, the lack of concurrency control allows for stealthy persistence after an initial compromise and facilitates parallel automated attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Are concurrent sessions limited by the application policy?
- [ ] Yes — only one session **is permitted** per user; new logins invalidate old ones *(Most secure)*  
- [ ] Yes — a fixed number of concurrent sessions **is enforced** and cannot be exceeded  
- [ ] No — an unlimited number of concurrent sessions **is possible**  

### How does the application respond when the session limit is reached?
- [ ] The oldest active session **is automatically terminated** (session fixation/hijacking mitigation)  
- [ ] The user **is prompted** to terminate existing sessions before the new session is authorized  
- [ ] The new login attempt **is blocked** until a manual logout occurs from another device  
- [ ] No action is taken — multiple sessions remain **enabled** simultaneously  

### Does the application provide a session management interface for the user?
- [ ] Yes — users **can** view active sessions (IP, device, time) and remotely terminate them  
- [ ] Yes — users **can** view active sessions but **cannot** terminate them remotely  
- [ ] No — users **cannot** see or manage other active sessions associated with their account  

### Is the user notified when concurrent login activity is detected?
- [ ] Yes — an alert (email, SMS, or in-app) **is triggered** for logins from new devices or locations  
- [ ] No — no notification **is provided** when concurrent sessions are established  

---