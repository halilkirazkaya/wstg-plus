## WSTG-INPV-15 — HTTP Splitting Smuggling

HTTP Splitting and Smuggling vulnerabilities arise from discrepancies in how frontend proxies and backend servers interpret and process HTTP request boundaries, specifically concerning `Content-Length` and `Transfer-Encoding` headers. By crafting ambiguous requests, an attacker can "smuggle" a hidden request to the backend or inject CRLF sequences to split a response, leading to cache poisoning, request hijacking, or security control bypass. These flaws typically manifest in complex environments using reverse proxies, load balancers, or CDNs that have inconsistent parsing logic. From an attacker's perspective, this allows for the redirection of user traffic, theft of sensitive session tokens, and the execution of unauthorized actions within the context of other users' sessions.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Is the environment susceptible to request smuggling via CL.TE or TE.CL discrepancies?
- [ ] No — frontend and backend servers handle request boundaries consistently  
- [ ] Yes — discrepancies exist but exploitation is **not possible** due to infrastructure mitigations  
- [ ] Yes — CL.TE or TE.CL smuggling **is possible**, allowing hidden requests to reach the backend *(High)*  
- [ ] Yes — TE.TE (double encoding/obfuscation) **is possible** to bypass frontend filters *(Critical)*  

### Can HTTP Response Splitting be achieved through CRLF injection in headers?
- [ ] No — application properly sanitizes CRLF sequences in all header inputs  
- [ ] Yes — CRLF sequences are reflected in headers but response splitting is **not possible**  
- [ ] Yes — CRLF injection **is possible**, allowing for header injection or cache poisoning  

### Are security controls (WAF/ACLs) bypassed using smuggled requests?
- [ ] No — security controls apply to both outer and smuggled requests  
- [ ] Yes — smuggled requests **can** bypass frontend WAF rules or IP-based ACLs  

### Is it possible to hijack other users' sessions or redirect their traffic?
- [ ] No — request/response streams are isolated and cannot be crossed  
- [ ] Yes — request smuggling **is possible** and allows capturing other users' requests *(Critical)*  
- [ ] Yes — response splitting **is possible** and allows for browser-side cache poisoning or XSS  

---