## WSTG-SESS-09 — Testing for Session Hijacking

Session hijacking occurs when an attacker captures, predicts, or fixes a valid session identifier (SID) to gain unauthorized access to a user's active session. This vulnerability typically manifests through insufficient transport layer security, lack of cookie security flags, or predictable SID generation algorithms that allow an attacker to bypass authentication entirely. From an attacker's perspective, successful exploitation provides full impersonation of the victim without requiring their credentials, often achieved through network sniffing, Cross-Site Scripting (XSS), or session fixation attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Are session identifiers protected during transit?
- [ ] Yes — `Secure` flag is **enabled** and HSTS is strictly enforced  
- [ ] Yes — `Secure` flag is **enabled** but HSTS is **disabled**  
- [ ] No — `Secure` flag is **not applied**, allowing SID interception over unencrypted channels *(High)*  

### Is the session identifier protected from client-side script access?
- [ ] Yes — `HttpOnly` flag is **applied** to all session-related cookies  
- [ ] No — `HttpOnly` flag is **not applied**, allowing SID exfiltration via XSS *(Critical)*  

### Does the application implement session binding to client attributes?
- [ ] Yes — session is bound to client attributes (IP/User-Agent) and reuse **not possible**  
- [ ] Yes — session binding is present but bypass **is possible** via header spoofing  
- [ ] No — no session binding exists; SID **is valid** when used from any source  

### Is the session identifier sufficiently random to prevent prediction?
- [ ] Yes — SID uses a cryptographically secure pseudo-random number generator (CSPRNG)  
- [ ] No — SID length is insufficient or follows a **predictable** sequence/pattern  

### Are concurrent sessions managed to limit the attack window?
- [ ] No — application strictly enforces a single active session per user  
- [ ] Yes — multiple concurrent sessions are **enabled** but monitored  
- [ ] Yes — unlimited concurrent sessions **are possible** across different devices/locations  

---