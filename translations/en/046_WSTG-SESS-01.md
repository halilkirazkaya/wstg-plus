## WSTG-SESS-01 — Testing for Session Management Schema

Session management schema testing focuses on analyzing how the application handles the lifecycle and structure of session identifiers to ensure they are robust against prediction and hijacking. Attackers capture multiple session tokens to perform statistical analysis, looking for patterns, low entropy, or predictable increments that allow them to forge valid sessions for other users. Weaknesses in the schema often manifest as session fixation vulnerabilities or the use of insufficiently random values, typically found in HTTP cookies, URL parameters, or custom header implementations. Compromising the session schema is a high-impact attack that grants an adversary complete access to a victim's authenticated state without needing their primary credentials.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Is the session identifier sufficiently complex and random?
- [ ] Yes — token passes statistical randomness tests (high entropy) and bypass **is not possible**  
- [ ] Yes — token is long but contains predictable patterns or static segments  
- [ ] No — token is sequential, based on predictable timestamps, or has **critically low** entropy *(Critical)*  

### Where is the session identifier transmitted and stored?
- [ ] Identifier is stored in a `Secure` and `HttpOnly` cookie *(Most secure)*  
- [ ] Identifier is stored in a cookie but lacks `Secure` or `HttpOnly` flags  
- [ ] Identifier is transmitted via URL parameters or `GET` requests **only** *(High Risk)*  

### Does the application issue a new session identifier upon successful authentication?
- [ ] Yes — a new, unique session ID **is generated** immediately after login  
- [ ] No — the session ID remains the same before and after login *(Session Fixation possible)*  

### Is the session identifier tied to a specific IP address or User-Agent to prevent reuse?
- [ ] Yes — session is invalidated if the client's fingerprint changes significantly  
- [ ] No — session **can** be reused across different IP addresses or devices without additional verification  

### Are session identifiers properly invalidated on the server-side after logout or timeout?
- [ ] Yes — server-side session state is **completely destroyed** upon logout  
- [ ] No — session remains valid on the server even if the client-side cookie is deleted  
- [ ] No — session **never expires** or has an excessively long timeout period  

---