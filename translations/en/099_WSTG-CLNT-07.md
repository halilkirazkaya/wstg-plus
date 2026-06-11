## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) is a browser-based mechanism that allows a server to explicitly permit access to its resources from different origins, effectively relaxing the Same-Origin Policy (SOP). Misconfigurations occur when an application overly trusts arbitrary origins, often by reflecting the `Origin` header in the `Access-Control-Allow-Origin` response header or by using poorly implemented regex whitelists. From an attacker's perspective, a permissive CORS policy can be exploited by hosting a malicious script on a controlled domain that makes authenticated requests to the vulnerable application, allowing the attacker to exfiltrate sensitive data, such as CSRF tokens or PII, directly from the response body. This vulnerability is most critical on API endpoints that process authenticated sessions and return non-public information.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Tools:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Does the application implement CORS headers on authenticated endpoints?
- [ ] No — CORS is **not enabled**; browser defaults to strict Same-Origin Policy  
- [ ] Yes — CORS headers are present and restricted to a strict **whitelist**  
- [ ] Yes — CORS headers are present but configured **permissively**  

### How does the server handle the `Access-Control-Allow-Origin` (ACAO) header?
- [ ] ACAO is set to a specific, **trusted** static domain  
- [ ] ACAO is set to `*` (wildcard) and credentials **cannot** be sent  
- [ ] ACAO is set to `*` (wildcard) and credentials **can** be sent *(Note: Most browsers block this)*  
- [ ] ACAO **dynamically reflects** the value of the `Origin` header from the request  

### Is the `Access-Control-Allow-Credentials` (ACAC) header set to `true`?
- [ ] No — ACAC is **not present** or set to `false`  
- [ ] Yes — ACAC is set to `true`, but ACAO is **restricted** to trusted origins  
- [ ] Yes — ACAC is set to `true` and ACAO **reflects** arbitrary origins *(Critical)*  

### Can the origin whitelist be bypassed via subdomain or null origin manipulation?
- [ ] No — whitelist is strictly enforced and **cannot** be bypassed  
- [ ] Yes — whitelist accepts **any** subdomain of the target (e.g., `attacker.target.com`)  
- [ ] Yes — whitelist accepts the `null` origin via sandboxed iframes  
- [ ] Yes — whitelist is vulnerable to regex bypasses (e.g., `target.com.attacker.com` or `target.com-safe.com`)  

### Does the CORS configuration allow sensitive data exfiltration?
- [ ] No — exposed endpoints do **not** contain sensitive or session-specific data  
- [ ] Yes — authenticated endpoints return PII, credentials, or CSRF tokens that **can** be read by an unauthorized origin  

---