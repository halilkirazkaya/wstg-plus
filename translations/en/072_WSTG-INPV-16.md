## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS) occurs when a chain of HTTP servers (such as a load balancer and a back-end web server) interprets the boundaries of a request differently, typically due to conflicting `Content-Length` and `Transfer-Encoding` headers. An attacker exploits this desynchronization to "smuggle" an entry into the back-end server's request buffer, effectively prefixing the next legitimate user's request with an attacker-controlled segment. This technique allows for severe attacks including session hijacking, bypassing security controls, and cache poisoning. Exploitation is usually performed by timing-based analysis or by observing unexpected responses from the back-end when subsequent requests are sent to the server.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Test Status** | Not Performed |
| **Severity** | Critical / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Do the front-end and back-end servers handle the `Transfer-Encoding: chunked` header consistently?
- [ ] Yes — both servers handle chunked encoding identically and desynchronization **not possible**  
- [ ] No — servers interpret headers differently but sanitization/normalization **is applied**  
- [ ] No — CL.TE (Content-Length/Transfer-Encoding) desynchronization **is possible** *(Critical)*  
- [ ] No — TE.CL (Transfer-Encoding/Content-Length) desynchronization **is possible** *(Critical)*  

### Can the attacker poison the server or client-side cache using a smuggled request?
- [ ] No — caching is **disabled** or not susceptible to poisoning  
- [ ] Yes — smuggled requests **can** redirect users to malicious domains or inject scripts via cache poisoning  

### Is it possible to capture other users' requests or session tokens via request concatenation?
- [ ] No — smuggling does **not** allow for cross-user data exposure  
- [ ] Yes — smuggling allows appending the next user's request to an attacker-controlled `POST` body, making exfiltration **possible**  

### Does the infrastructure use HTTP/2, and is it susceptible to H2.CL or H2.TE desynchronization?
- [ ] No — application uses HTTP/1.1 only or HTTP/2 is implemented securely without downgrading  
- [ ] Yes — HTTP/2 to HTTP/1.1 cleartext downgrades occur and desynchronization **is possible**  

### Are malformed headers (e.g., `Transfer-Encoding:  chunked` with a space) handled securely?
- [ ] Yes — malformed headers are rejected or normalized across all tiers  
- [ ] No — TE.TE desynchronization **is possible** due to inconsistent handling of malformed or obscured headers  

---